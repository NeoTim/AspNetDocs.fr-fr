---
ms.openlocfilehash: a0546c44284a78ce0e8d06b0f2b9f65ecf66fac7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031686"
---

L’annotation de données `[Column(TypeName = "decimal(18, 2)")]` est nécessaire pour qu’Entity Framework Core puisse correctement mapper `Price` en devise dans la base de données. Pour plus d’informations, consultez [Types de données](/ef/core/modeling/relational/data-types).

Le modèle terminé :

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1)]

[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) est abordé dans le tutoriel suivant. L’attribut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) spécifie les éléments à afficher pour le nom d’un champ (dans le cas présent, « Release Date » au lieu de « ReleaseDate »). L’attribut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) spécifie le type des données (Date). Les informations d’heures stockées dans le champ ne s’affichent donc pas.

Accédez à Pages/Movies, puis placez le curseur sur un lien **Modifier** pour afficher l’URL cible.

![Fenêtre de navigateur avec la souris sur le lien Edit et l’URL de lien http://localhost:1234/Movies/Edit/5 affichée](~/tutorials/razor-pages/da1/edit7.png)

Les liens **Edit**, **Details**, et **Delete** sont générés par le [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) dans le fichier *Pages/Movies/Index.cshtml*.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor. Dans le code précédent, `AnchorTagHelper` génère dynamiquement la valeur d’attribut HTML `href` dans la page Razor (l’itinéraire est relatif), `asp-page` et l’ID de l’itinéraire (`asp-route-id`). Pour plus d’informations, consultez [Génération d’URL pour les pages](xref:razor-pages/index#url-generation-for-pages).

Utilisez **Afficher la Source** dans votre navigateur favori pour examiner le balisage généré. Une partie du code HTML généré est affichée ci-dessous :

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Les liens générés dynamiquement transmettent l’ID de film avec une chaîne de requête (par exemple, `http://localhost:5000/Movies/Details?id=2`).

Mettez à jour les pages Razor Edit, Details et Delete pour utiliser le modèle d’itinéraire « {id:int} ». Remplacez la directive de chacune de ces pages (`@page`) par `@page "{id:int}"`. Exécuter l’application, puis affichez le code source. Le code HTML généré ajoute l’ID à la partie de chemin de l’URL :

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Une requête à la page avec le modèle d’itinéraire « {id:int} » qui n’inclut **pas** l’entier retourne une erreur HTTP 404 (introuvable). Par exemple, `http://localhost:5000/Movies/Details` retourne une erreur 404. Pour que l’ID soit facultatif, ajoutez `?` à la contrainte d’itinéraire :

 ```cshtml
@page "{id:int?}"
```

::: moniker range="= aspnetcore-2.0"

### <a name="update-concurrency-exception-handling"></a>Mettre à jour la gestion des exceptions d’accès concurrentiel

Mettez à jour la méthode `OnPostAsync` dans le fichier *Pages/Movies/Edit.cshtml.cs*. Le code en surbrillance suivant illustre les changements :

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

Le code précédent détecte uniquement les exceptions d’accès concurrentiel quand le premier client simultané supprime le film et que le deuxième client simultané poste les modifications apportées au film.

Pour tester le bloc `catch` :

* Définissez un point d'arrêt sur `catch (DbUpdateConcurrencyException)`
* Modifiez un film.
* Dans une autre fenêtre de navigateur, sélectionnez le lien **Delete** du même film, puis supprimez le film.
* Dans la fenêtre de navigateur précédente, postez les modifications apportées au film.

Le code de production détecte généralement les conflits d’accès concurrentiel quand deux clients, ou plus, mettent simultanément à jour un enregistrement. Pour plus d’informations, consultez [Gérer les conflits d’accès concurrentiel](xref:data/ef-rp/concurrency).

### <a name="posting-and-binding-review"></a>Validation de la publication et de la liaison

Examinez le fichier *Pages/Movies/Edit.cshtml.cs* :

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

Quand une requête HTTP GET est effectuée sur la page Movies/Edit (par exemple, `http://localhost:5000/Movies/Edit/2`) :

* La méthode `OnGetAsync` extrait le film de la base de données et retourne la méthode `Page`. 
* La méthode `Page` restitue la page Razor *Pages/Movies/Edit.cshtml*. Le fichier *Pages/Movies/Edit.cshtml* contient la directive de modèle (`@model RazorPagesMovie.Pages.Movies.EditModel`), ce qui rend le modèle Movie disponible sur la page.
* Le formulaire d’édition s’affiche avec les valeurs relatives au film.

Quand la page Movies/Edit est postée :

* Les valeurs de formulaire affichées dans la page sont liées à la propriété `Movie`. L’attribut `[BindProperty]` active la [liaison de données](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* S’il existe des erreurs dans l’état du modèle (par exemple, si `ReleaseDate` ne peut pas être converti en date), le formulaire est à nouveau posté avec les valeurs soumises.
* S’il n’y a aucune erreur de modèle, le film est enregistré.

Les méthodes HTTP GET dans les pages Razor Index, Create et Delete suivent un modèle similaire. La méthode HTTP POST `OnPostAsync` dans la page Razor Create suit un modèle semblable à la méthode `OnPostAsync` dans la page Razor Edit.

La fonction de recherche est ajoutée dans le prochain didacticiel.
