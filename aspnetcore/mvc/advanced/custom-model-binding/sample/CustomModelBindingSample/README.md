---
ms.openlocfilehash: 71a40fcab8f1d1005cbe9327d01a42484765a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059636"
---
# <a name="custom-model-binding-demo"></a>Démonstration de liaison de modèles personnalisée

Testez `ByteArrayModelBinder` en exécutant l’application et en effectuant un POST d’une chaîne encodée en base 64 sur le point de terminaison `ImageController` (`/api/image/`). Spécifiez les propriétés de nom et de nom de fichier dans le corps de la requête en tant que form-data (à l’aide de [Postman](https://www.getpostman.com/) ou d’un outil similaire). Vous pouvez utiliser [cet exemple de chaîne](Base64String.txt). Le résultat est enregistré dans le dossier *wwwroot/images/upload* avec le nom de fichier spécifié.

Pour tester l’exemple de liaison personnalisée, essayez les points de terminaison suivants :

* /api/authors/1
* /api/authors/2 (INTROUVABLE)
* /api/boundauthors/1
* /api/boundauthors/2 (INTROUVABLE)
* /api/boundauthors/get/1
* /api/boundauthors/get/2 (AUCUN CONTENU) &ndash; Cette action ne vérifie pas la présence d’une valeur null et retourne une erreur de type *404 Introuvable*.
