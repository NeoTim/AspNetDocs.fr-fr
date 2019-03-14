---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044076"
---
## <a name="add-initial-migration-and-update-the-database"></a><span data-ttu-id="458fc-101">Ajoutez une migration initiale et de mettre à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="458fc-101">Add initial migration and update the database</span></span>

* <span data-ttu-id="458fc-102">Ouvrez une invite de commandes et accédez au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="458fc-102">Open a command prompt and navigate to the project directory.</span></span> <span data-ttu-id="458fc-103">(Le répertoire contenant le *Startup.cs* fichier).</span><span class="sxs-lookup"><span data-stu-id="458fc-103">(The directory containing the *Startup.cs* file).</span></span>

* <span data-ttu-id="458fc-104">À l’invite de commandes, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="458fc-104">Run the following commands in the command prompt:</span></span>

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  <span data-ttu-id="458fc-105">[.NET core](/dotnet/core/tools/index) est une implémentation multiplateforme de .NET.</span><span class="sxs-lookup"><span data-stu-id="458fc-105">[.NET Core](/dotnet/core/tools/index) is a cross-platform implementation of .NET.</span></span> <span data-ttu-id="458fc-106">Voici ce que faire ces commandes :</span><span class="sxs-lookup"><span data-stu-id="458fc-106">Here is what these commands do:</span></span>

  * <span data-ttu-id="458fc-107">[restauration de dotnet](/dotnet/core/tools/dotnet-restore): Télécharge les packages NuGet spécifiés dans le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="458fc-107">[dotnet restore](/dotnet/core/tools/dotnet-restore): Downloads the NuGet packages specified in the *.csproj* file.</span></span>
  * <span data-ttu-id="458fc-108">`dotnet ef migrations add Initial` Exécute la commande de migrations Entity Framework .NET Core CLI et crée la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="458fc-108">`dotnet ef migrations add Initial` Runs the Entity Framework .NET Core CLI migrations command and creates the initial migration.</span></span> <span data-ttu-id="458fc-109">Le paramètre après « ajouter » est un nom que vous attribuez à la migration.</span><span class="sxs-lookup"><span data-stu-id="458fc-109">The parameter after "add" is a name that you assign to the migration.</span></span> <span data-ttu-id="458fc-110">Ici vous nommez la migration « Initial » car il s’agit de la migration de base de données initiale.</span><span class="sxs-lookup"><span data-stu-id="458fc-110">Here you're naming the migration "Initial" because it's the initial database migration.</span></span> <span data-ttu-id="458fc-111">Cette opération crée le *Migrations de données / /\<date-heure > _Initial.cs* fichier contenant les commandes de migration pour ajouter le *film* table à la base de données.</span><span class="sxs-lookup"><span data-stu-id="458fc-111">This operation creates the *Data/Migrations/\<date-time>_Initial.cs* file containing the migration commands to add the *Movie* table to the database.</span></span>
  * <span data-ttu-id="458fc-112">`dotnet ef database update`  Met à jour la base de données avec la migration que nous venons de créer.</span><span class="sxs-lookup"><span data-stu-id="458fc-112">`dotnet ef database update`  Updates the database with the migration we just created.</span></span>

<span data-ttu-id="458fc-113">Vous allez en savoir plus sur la chaîne de connexion et de la base de données dans le didacticiel suivant.</span><span class="sxs-lookup"><span data-stu-id="458fc-113">You'll learn about the database and connection string in the next tutorial.</span></span> <span data-ttu-id="458fc-114">Vous allez découvrir les modifications du modèle de données dans le [ajouter un champ](xref:tutorials/first-mvc-app/new-field) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="458fc-114">You'll learn about data model changes in the [Add a field](xref:tutorials/first-mvc-app/new-field) tutorial.</span></span>
