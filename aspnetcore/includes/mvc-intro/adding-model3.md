---
ms.openlocfilehash: 3940675548ad7496aab9c720ee0b7fd512bfe029
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043456"
---

## <a name="test-the-app"></a>Tester l’application

* Exécutez l’application et appuyez sur le lien **Mvc Movie**.
* Appuyez sur le lien **Create New** et créez un film.

  ![Créer une vue avec des champs pour le genre, le prix, la date de sortie et le titre](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* Vous ne pourrez peut-être pas entrer des séparateurs décimaux ou des virgules dans le champ `Price`. Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (« , ») comme décimale et des formats de date autres que l’anglais des États-Unis, vous devez effectuer des étapes pour localiser votre application. Pour plus d’informations, consultez [https://github.com/aspnet/Docs/issues/4076](https://github.com/aspnet/Docs/issues/4076) et [Ressources supplémentaires](#additional-resources). Pour le moment, entrez simplement des nombres entiers tels que 10.

<a name="displayformatdatelocal"></a>

* Avec certains paramètres régionaux, vous devez spécifier le format de date. Consultez le code en surbrillance ci-dessous.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Nous parlerons de `DataAnnotations` plus loin dans le didacticiel.

Quand vous appuyez sur **Create**, le formulaire est envoyé au serveur, où les informations sur le film sont enregistrées dans une base de données. L’application redirige vers l’URL */Movies*, où les nouvelles informations sur le film sont affichées.

![Vue de film montrant une nouvelle entrée](~/tutorials/first-mvc-app/adding-model/_static/h.png)

Créez deux ou trois autres entrées. Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.
