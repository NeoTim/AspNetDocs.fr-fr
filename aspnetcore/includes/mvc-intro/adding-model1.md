---
ms.openlocfilehash: 72e33ea44976963193d2560427fc418875be450e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038866"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Ajouter un modèle dans une application ASP.NET Core MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/tdykstra)

Dans cette section, vous ajoutez des classes pour la gestion des films d’une base de données. Ces classes constituent la partie « **M**odèle » de l’application **M**VC.

Vous utilisez ces classes avec [Entity Framework Core](/ef/core) (EF Core) pour travailler avec une base de données. EF Core est un framework de mappage relationnel d’objets qui simplifie le code d’accès aux données à écrire. [EF Core prend en charge de nombreux moteurs de base de données](/ef/core/providers/).

Les classes de modèle que vous créez portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core. Elles définissent simplement les propriétés des données stockées dans la base de données.

Dans ce didacticiel, vous écrivez d’abord les classes du modèle, puis EF Core crée la base de données. Une autre approche que nous ne décrivons pas ici consiste à générer les classes du modèle à partir d’une base de données déjà existante. Pour plus d’informations sur cette approche, consultez [ASP.NET Core - Base de données existante](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Ajouter une classe de modèle de données
