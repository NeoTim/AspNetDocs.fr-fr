---
ms.openlocfilehash: 71a40fcab8f1d1005cbe9327d01a42484765a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059636"
---
# <a name="custom-model-binding-demo"></a><span data-ttu-id="d40b9-101">Démonstration de liaison de modèles personnalisée</span><span class="sxs-lookup"><span data-stu-id="d40b9-101">Custom Model Binding Demo</span></span>

<span data-ttu-id="d40b9-102">Testez `ByteArrayModelBinder` en exécutant l’application et en effectuant un POST d’une chaîne encodée en base 64 sur le point de terminaison `ImageController` (`/api/image/`).</span><span class="sxs-lookup"><span data-stu-id="d40b9-102">Test `ByteArrayModelBinder` by running the app and POSTing a base64-encoded string to the `ImageController` endpoint (`/api/image/`).</span></span> <span data-ttu-id="d40b9-103">Spécifiez les propriétés de nom et de nom de fichier dans le corps de la requête en tant que form-data (à l’aide de [Postman](https://www.getpostman.com/) ou d’un outil similaire).</span><span class="sxs-lookup"><span data-stu-id="d40b9-103">Specify the file and filename properties in the request body as form-data (using [Postman](https://www.getpostman.com/) or a similar tool).</span></span> <span data-ttu-id="d40b9-104">Vous pouvez utiliser [cet exemple de chaîne](Base64String.txt).</span><span class="sxs-lookup"><span data-stu-id="d40b9-104">You can use [this sample string](Base64String.txt).</span></span> <span data-ttu-id="d40b9-105">Le résultat est enregistré dans le dossier *wwwroot/images/upload* avec le nom de fichier spécifié.</span><span class="sxs-lookup"><span data-stu-id="d40b9-105">The result is saved in the *wwwroot/images/upload* folder with the filename specified.</span></span>

<span data-ttu-id="d40b9-106">Pour tester l’exemple de liaison personnalisée, essayez les points de terminaison suivants :</span><span class="sxs-lookup"><span data-stu-id="d40b9-106">To test the custom binding example, try the following endpoints:</span></span>

* <span data-ttu-id="d40b9-107">/api/authors/1</span><span class="sxs-lookup"><span data-stu-id="d40b9-107">/api/authors/1</span></span>
* <span data-ttu-id="d40b9-108">/api/authors/2 (INTROUVABLE)</span><span class="sxs-lookup"><span data-stu-id="d40b9-108">/api/authors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="d40b9-109">/api/boundauthors/1</span><span class="sxs-lookup"><span data-stu-id="d40b9-109">/api/boundauthors/1</span></span>
* <span data-ttu-id="d40b9-110">/api/boundauthors/2 (INTROUVABLE)</span><span class="sxs-lookup"><span data-stu-id="d40b9-110">/api/boundauthors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="d40b9-111">/api/boundauthors/get/1</span><span class="sxs-lookup"><span data-stu-id="d40b9-111">/api/boundauthors/get/1</span></span>
* <span data-ttu-id="d40b9-112">/api/boundauthors/get/2 (AUCUN CONTENU) &ndash; Cette action ne vérifie pas la présence d’une valeur null et retourne une erreur de type *404 Introuvable*.</span><span class="sxs-lookup"><span data-stu-id="d40b9-112">/api/boundauthors/get/2 (NO CONTENT) &ndash; This action doesn't check for null and returns a *404 Not Found*.</span></span>
