---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Création d’une chaîne de connexion et utilisation de SQL Server base de données locale | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 4400cb8c6a5d57da60d164220f929d69d7f4d2f7
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044257"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="984d0-102">Création d’une chaîne de connexion et utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="984d0-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="984d0-103">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="984d0-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="984d0-104">Création d’une chaîne de connexion et utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="984d0-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="984d0-105">La `MovieDBContext` classe que vous avez créée gère la tâche de connexion à la base de données et de mappage `Movie` d’objets aux enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="984d0-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="984d0-106">L’une des questions que vous pouvez poser, cependant, est la façon de spécifier la base de données à laquelle elle doit se connecter.</span><span class="sxs-lookup"><span data-stu-id="984d0-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="984d0-107">Vous n’avez pas besoin de spécifier la base de données à utiliser, [Entity Framework utilisera](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)par défaut la base de données locale.</span><span class="sxs-lookup"><span data-stu-id="984d0-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="984d0-108">Dans cette section, nous allons ajouter de manière explicite une chaîne de connexion dans le fichier *Web.config* de l’application.</span><span class="sxs-lookup"><span data-stu-id="984d0-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="984d0-109">Base de données locale SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="984d0-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="984d0-110">La base de données [locale est une](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) version allégée de l’SQL Server Express moteur de base de données qui démarre à la demande et s’exécute en mode utilisateur.</span><span class="sxs-lookup"><span data-stu-id="984d0-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="984d0-111">La base de données locale s’exécute dans un mode d’exécution spécial de SQL Server Express qui vous permet d’utiliser des bases de données en tant que fichiers *. mdf* .</span><span class="sxs-lookup"><span data-stu-id="984d0-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="984d0-112">En règle générale, les fichiers de base de données de base de données locale sont conservés dans le dossier *application \_ Data* d’un projet Web.</span><span class="sxs-lookup"><span data-stu-id="984d0-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="984d0-113">SQL Server Express n’est pas recommandé pour une utilisation dans les applications Web de production.</span><span class="sxs-lookup"><span data-stu-id="984d0-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="984d0-114">En particulier, la base de données locale ne doit pas être utilisée pour la production avec une application Web, car elle n’est pas conçue pour fonctionner avec IIS.</span><span class="sxs-lookup"><span data-stu-id="984d0-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="984d0-115">Toutefois, une base de données de base de données locale peut être facilement migrée vers SQL Server ou SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="984d0-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="984d0-116">Dans Visual Studio 2017, la base de données locale est installée par défaut dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="984d0-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="984d0-117">Par défaut, le Entity Framework recherche une chaîne de connexion nommée identique à la classe de contexte de l’objet ( `MovieDBContext` pour ce projet).</span><span class="sxs-lookup"><span data-stu-id="984d0-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="984d0-118">Pour plus d’informations, consultez [SQL Server des chaînes de connexion pour les applications Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="984d0-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="984d0-119">Ouvrez le fichier de *Web.config* racine de l’application, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="984d0-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="984d0-120">(Pas le fichier *Web.config* dans le dossier *views* .)</span><span class="sxs-lookup"><span data-stu-id="984d0-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="984d0-121">Recherchez l' `<connectionStrings>` élément :</span><span class="sxs-lookup"><span data-stu-id="984d0-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="984d0-122">Ajoutez la chaîne de connexion suivante à l' `<connectionStrings>` élément dans le fichier *Web.config* .</span><span class="sxs-lookup"><span data-stu-id="984d0-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="984d0-123">L’exemple suivant montre une partie du fichier *Web.config* avec la nouvelle chaîne de connexion ajoutée :</span><span class="sxs-lookup"><span data-stu-id="984d0-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="984d0-124">Les deux chaînes de connexion sont très similaires.</span><span class="sxs-lookup"><span data-stu-id="984d0-124">The two connection strings are very similar.</span></span> <span data-ttu-id="984d0-125">La première chaîne de connexion est nommée `DefaultConnection` et utilisée pour la base de données d’appartenance afin de contrôler qui peut accéder à l’application.</span><span class="sxs-lookup"><span data-stu-id="984d0-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="984d0-126">La chaîne de connexion que vous avez ajoutée spécifie une base de données de base de données locale nommée *Movie. mdf* située dans le dossier *application \_ Data* .</span><span class="sxs-lookup"><span data-stu-id="984d0-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="984d0-127">Nous n’utiliserons pas la base de données d’appartenance dans ce didacticiel. pour plus d’informations sur l’appartenance, l’authentification et la sécurité, consultez mon didacticiel [créer une application ASP.NET MVC avec auth et SQL DB et déployer sur Azure App service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="984d0-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="984d0-128">Le nom de la chaîne de connexion doit correspondre au nom de la classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .</span><span class="sxs-lookup"><span data-stu-id="984d0-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="984d0-129">Vous n’avez pas besoin d’ajouter la `MovieDBContext` chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="984d0-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="984d0-130">Si vous ne spécifiez pas de chaîne de connexion, Entity Framework créera une base de données de base de données locale dans le répertoire Users avec le nom complet de la classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (dans ce cas `MvcMovie.Models.MovieDBContext` ).</span><span class="sxs-lookup"><span data-stu-id="984d0-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="984d0-131">Vous pouvez nommer la base de données comme vous le souhaitez, tant qu’elle a la valeur \*. \* Suffixe MDF.</span><span class="sxs-lookup"><span data-stu-id="984d0-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="984d0-132">Par exemple, nous pourrions nommer la base de données *MyFilms. mdf*.</span><span class="sxs-lookup"><span data-stu-id="984d0-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="984d0-133">Ensuite, vous allez générer une nouvelle `MoviesController` classe que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer des listes de films.</span><span class="sxs-lookup"><span data-stu-id="984d0-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="984d0-134">[Précédent](adding-a-model.md) 
>  [Suivant](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="984d0-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
