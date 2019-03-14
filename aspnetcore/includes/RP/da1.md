---
ms.openlocfilehash: e5565381f44480e2531925717ee7815da92d1ff5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030036"
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="9aae2-101">Mettre à jour les pages générées</span><span class="sxs-lookup"><span data-stu-id="9aae2-101">Update the generated pages</span></span>

<span data-ttu-id="9aae2-102">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9aae2-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9aae2-103">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="9aae2-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="9aae2-104">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être **Release Date** (en deux mots).</span><span class="sxs-lookup"><span data-stu-id="9aae2-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="9aae2-106">Mettre à jour le code généré</span><span class="sxs-lookup"><span data-stu-id="9aae2-106">Update the generated code</span></span>

<span data-ttu-id="9aae2-107">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes affichées en surbrillance dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="9aae2-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
