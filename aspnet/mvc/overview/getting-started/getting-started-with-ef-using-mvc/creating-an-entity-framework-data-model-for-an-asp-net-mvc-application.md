---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Tutoriel : Bien démarrer avec Entity Framework 6 Code First avec MVC 5 | Microsoft Docs'
description: Dans cette série de didacticiels, vous allez apprendre à créer une application ASP.NET MVC 5 qui utilise Entity Framework 6 pour accéder aux données.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57057966"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Tutoriel : Bien démarrer avec Entity Framework 6 Code First avec MVC 5

> [!NOTE]
> Pour tout nouveau développement, nous vous recommandons de [ASP.NET Core Razor Pages](/aspnet/core/razor-pages) sur les contrôleurs MVC ASP.NET et des vues. Pour une série de didacticiels semblable à celui-ci à l’aide de Pages Razor, consultez [didacticiel : Bien démarrer avec les Pages Razor dans ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). Le nouveau didacticiel :
> * est plus facile à suivre ;
> * Fournit d’autres bonnes pratiques sur EF Core.
> * Utilise des requêtes plus efficaces.
> * Est plus à jour avec la dernière API.
> * couvre davantage de fonctionnalités ;
> * représente la meilleure approche pour le développement de nouvelles applications.

Dans cette série de didacticiels, vous allez apprendre à créer une application ASP.NET MVC 5 qui utilise Entity Framework 6 pour accéder aux données. Ce didacticiel utilise le flux de travail Code First. Pour plus d’informations sur le choix entre Code First, Model First et Database First, consultez [créer un modèle](/ef/ef6/modeling/).

Cette série de didacticiels explique comment générer l’exemple d’application Contoso University. L’exemple d’application est un site Web simple d’université. Avec elle, vous pouvez afficher et mettre à jour des étudiants, les cours et les informations relatives aux enseignants. Voici deux des écrans que vous créez :

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Modifier un étudiant](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créer une application web MVC
> * Configurer le style du site
> * Installer Entity Framework 6
> * Créer le modèle de données
> * Créer le contexte de base de données
> * Initialiser la base de données avec des données de test
> * Configuration d’EF 6 pour utiliser la base de données locale
> * Créer un contrôleur et des vues
> * Afficher la base de données

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Créer une application web MVC

1. Ouvrez Visual Studio et créez un C# projet web à l’aide du **Application Web ASP.NET (.NET Framework)** modèle. Nommez le projet *ContosoUniversity* et sélectionnez **OK**.

   ![Boîte de dialogue Nouveau projet dans Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. Dans **nouvelle Application Web ASP.NET - ContosoUniversity**, sélectionnez **MVC**.

   ![Nouvelle boîte de dialogue d’application web dans Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > Par défaut, le **authentification** option est définie sur **aucune authentification**. Pour ce didacticiel, l’application web ne nécessite pas les utilisateurs à se connecter. En outre, il ne limite pas l’accès basé sur les clients connectés.

1. Sélectionnez **OK** pour créer le projet.

## <a name="set-up-the-site-style"></a>Configurer le style du site

Quelques changements simples configureront le menu, la disposition et la page d’accueil du site.

1. Ouvrez *Views\Shared\\_Layout.cshtml*et apportez les modifications suivantes :

   - Remplacez chaque occurrence de « My Application ASP.NET » et « Nom de l’Application » à « Contoso University ».
   - Ajouter des entrées de menu pour les étudiants, des cours, des formateurs et des services et supprimez l’entrée de Contact.

   Les modifications sont mises en surbrillance dans l’extrait de code suivant :

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. Dans *Views\Home\Index.cshtml*, remplacez le contenu du fichier par le code suivant pour remplacer le texte sur ASP.NET et MVC avec le texte de cette application :

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Appuyez sur Ctrl + F5 pour exécuter le site web. Vous consultez la page d’accueil avec le menu principal.

## <a name="install-entity-framework-6"></a>Installer Entity Framework 6

1. À partir de la **outils** menu, choisissez **Gestionnaire de Package NuGet**, puis choisissez **Console du Gestionnaire de Package**.

2. Dans le **Console du Gestionnaire de Package** fenêtre, entrez la commande suivante :

   ```text
   Install-Package EntityFramework
   ```

Cette étape est une des quelques étapes ayant ce didacticiel vous faire manuellement, mais qui aurait pu être fait automatiquement par la fonctionnalité de génération de modèles automatique ASP.NET MVC. Vous êtes les effectuer manuellement afin que vous puissiez voir les étapes requises pour utiliser Entity Framework (EF). Vous utiliserez la structure ultérieurement pour créer le contrôleur MVC et les vues. Une alternative consiste à laisser la structure automatiquement installer le package NuGet d’EF, créer la classe de contexte de base de données et créer la chaîne de connexion. Lorsque vous êtes prêt à le faire de cette façon, il vous suffit est ignorer ces étapes et de structurer votre contrôleur MVC après avoir créé vos classes d’entité.

## <a name="create-the-data-model"></a>Créer le modèle de données

Ensuite, vous allez créer des classes d’entités pour l’application Contoso University. Vous commencerez par les trois entités suivantes :

**Cours** <-> **inscription** <-> **étudiant**

| Entités | Relationship |
| -------- | ------------ |
| Cours pour l’inscription | Un-à-plusieurs |
| Étudiant à l’inscription | Un-à-plusieurs |

Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`, et une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. En d’autres termes, un étudiant peut être inscrit dans un nombre quelconque de cours et un cours peut avoir un nombre quelconque d’élèves inscrits.

Dans les sections suivantes, vous allez créer une classe pour chacune de ces entités.

> [!NOTE]
> Si vous essayez de compiler le projet avant d’avoir créé toutes ces classes d’entité, vous obtiendrez des erreurs de compilateur.

### <a name="the-student-entity"></a>L’entité Student

- Dans le *modèles* dossier, créez un fichier de classe nommé *Student.cs* en cliquant sur le dossier dans **l’Explorateur de solutions** et en choisissant **ajouter**  >  **Classe**. Remplacez le code du modèle par le code suivant :

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe. Par défaut, Entity Framework interprète une propriété nommée `ID` ou *classname* `ID` comme clé primaire.

La propriété `Enrollments` est une *propriété de navigation*. Les propriétés de navigation contiennent d’autres entités qui sont associées à cette entité. Dans ce cas, le `Enrollments` propriété d’un `Student` entité contiendra tous le `Enrollment` entités associées à ce `Student` entité. En d’autres termes, si une donnée `Student` ligne dans la base de données comporte deux liées `Enrollment` lignes (valeur des lignes qui contiennent la clé primaire de cette étudiant dans leurs `StudentID` colonne clé étrangère), qui `Student` l’entité `Enrollments` propriété de navigation contiendra ces deux `Enrollment` entités.

Propriétés de navigation sont généralement définies en tant que `virtual` afin qu’ils peuvent tirer parti de certaines fonctionnalités Entity Framework comme *le chargement différé*. (Le chargement différé est expliqué plus loin, dans le [lecture de données connexes](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) didacticiel plus loin dans cette série.)

Si une propriété de navigation peut contenir plusieurs entités (comme dans des relations plusieurs à plusieurs ou un -à-plusieurs), son type doit être une liste dans laquelle les entrées peuvent être ajoutées, supprimées et mises à jour, telle que `ICollection`.

### <a name="the-enrollment-entity"></a>L’entité Enrollment

- Dans le dossier *Models*, créez *Enrollment.cs* et remplacez le code existant par le code suivant :

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Le `EnrollmentID` propriété sera la clé primaire ; cette entité utilise le *classname* `ID` de modèle au lieu de `ID` par lui-même, comme vous l’avez vu dans la `Student` entité. En général, vous choisissez un modèle et l’utilisez dans tout votre modèle de données. Ici, la variante illustre que vous pouvez utiliser l’un ou l’autre modèle. Dans un prochain didacticiel, vous verrez comment l’utilisation `ID` sans `classname` rend plus facile à implémenter l’héritage dans le modèle de données.

Le `Grade` propriété est un [enum](/ef/ef6/modeling/code-first/data-types/enums). Le point d’interrogation après la `Grade` déclaration de type indique que le `Grade` propriété est [nullable](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Un niveau qui a la valeur null est différent à partir d’une note égale à zéro, cela signifie qu’une note n’est pas connue ou n’a pas encore été affectée.

La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`. Une entité `Enrollment` est associée à une entité `Student`, donc la propriété peut contenir uniquement une entité `Student` unique (contrairement à la propriété de navigation `Student.Enrollments` que vous avez vue précédemment, qui peut contenir plusieurs entités `Enrollment`).

La propriété `CourseID` est une clé étrangère et la propriété de navigation correspondante est `Course`. Une entité `Enrollment` est associée à une entité `Course`.

Entity Framework interprète une propriété comme une propriété de clé étrangère si elle se nomme *&lt;nom de propriété de navigation&gt;&lt;nom de propriété de clé primaire&gt;* (par exemple, `StudentID`pour le `Student` propriété de navigation depuis le `Student` clé primaire de l’entité est `ID`). Propriétés de clé étrangère peuvent également être nommées les mêmes simplement *&lt;nom de propriété de clé primaire&gt;* (par exemple, `CourseID` dans la mesure où le `Course` clé primaire de l’entité est `CourseID`).

### <a name="the-course-entity"></a>L’entité Course

- Dans le *modèles* dossier, créez *Course.cs*, en remplaçant le code du modèle par le code suivant :

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propriété `Enrollments` est une propriété de navigation. Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.

Nous fournirons plus de détails sur la <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> attribut dans un didacticiel plus loin dans cette série. En fait, cet attribut vous permet d’entrer la clé primaire pour le cours plutôt que de laisser la base de données la générer.

## <a name="create-the-database-context"></a>Créer le contexte de base de données

La classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié est le *contexte de base de données* classe. Vous créez cette classe en dérivant le [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) classe. Dans votre code, vous spécifiez les entités qui sont incluses dans le modèle de données. Vous pouvez également personnaliser un certain comportement d’Entity Framework. Dans ce projet, la classe est nommée `SchoolContext`.

- Pour créer un dossier dans le projet ContosoUniversity, cliquez sur le projet dans **l’Explorateur de solutions** et cliquez sur **ajouter**, puis cliquez sur **nouveau dossier**. Nommez le nouveau dossier *DAL* (pour la couche d’accès aux données). Dans ce dossier, créez un nouveau fichier de classe nommé *SchoolContext.cs*et remplacez le code du modèle par le code suivant :

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Spécifier des jeux d’entités

Ce code crée un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) propriété pour chaque jeu d’entités. Dans la terminologie Entity Framework, un *jeu d’entités* correspond généralement à une table de base de données et un *entité* correspond à une ligne dans la table.

> [!NOTE]
>
> Vous pouvez omettre le `DbSet<Enrollment>` et `DbSet<Course>` instructions et elle fonctionnerait de la même. Entity Framework les inclurait implicitement, car le `Student` références d’entité le `Enrollment` entité et la `Enrollment` références d’entité le `Course` entité.

### <a name="specify-the-connection-string"></a>Spécifiez la chaîne de connexion

Le nom de la chaîne de connexion (que vous allez ajouter au fichier Web.config plus loin) est transmis au constructeur.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Vous pouvez également transmettre la chaîne de connexion elle-même au lieu du nom d’un qui est stocké dans le fichier Web.config. Pour plus d’informations sur les options permettant de spécifier la base de données à utiliser, consultez [chaînes de connexion et de modèles](/ef/ef6/fundamentals/configuring/connection-strings).

Si vous ne spécifiez pas une chaîne de connexion ou le nom d’une manière explicite, Entity Framework suppose que le nom de chaîne de connexion est le même que le nom de classe. Le nom de chaîne de connexion par défaut dans cet exemple serait alors `SchoolContext`, identique à ce que vous spécifiez explicitement.

### <a name="specify-singular-table-names"></a>Spécifiez les noms de tables au singulier

Le `modelBuilder.Conventions.Remove` instruction dans le [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) méthode empêche l’en cours mis au pluriel des noms de table. Si vous n’avez pas le faire, les tables générées dans la base de données seraient nommés `Students`, `Courses`, et `Enrollments`. Au lieu de cela, les noms de table seront `Student`, `Course`, et `Enrollment`. Les développeurs ne sont pas tous d’accord sur la nécessité d’utiliser des noms de table au pluriel. Ce didacticiel utilise la forme au singulier, mais le point important est que vous pouvez sélectionner quelle que soit la forme que vous préférez en incluant ou l’omission de cette ligne de code.

## <a name="initialize-db-with-test-data"></a>Initialiser la base de données avec des données de test

Entity Framework peut automatiquement créer (ou supprimer et recréer) une base de données pour vous lors de l’application s’exécute. Vous pouvez spécifier qu’il doit être effectuée chaque fois que votre application s’exécute ou seulement lorsque le modèle est désynchronisé avec la base de données existante. Vous pouvez également écrire un `Seed` méthode que Entity Framework appelle automatiquement après avoir créé la base de données pour la remplir avec les données de test.

Le comportement par défaut consiste à créer une base de données uniquement si elle n’existe pas (et lever une exception si le modèle a changé et que la base de données existe déjà). Dans cette section, vous allez spécifier que la base de données doit être supprimée et recréée chaque fois que le modèle change. Suppression de la base de données entraîne la perte de toutes vos données. Il s’agit généralement OK pendant le développement, car la `Seed` méthode s’exécute lorsque la base de données est recréée et recrée vos données de test. Mais en production vous souhaitez généralement perdre toutes vos données chaque fois que vous devez modifier le schéma de base de données. Plus tard, vous verrez comment gérer les modifications apportées au modèle à l’aide des Migrations Code First pour modifier le schéma de base de données au lieu de devoir supprimer et recréer la base de données.

1. Dans le dossier de la couche DAL, créez un nouveau fichier de classe nommé *SchoolInitializer.cs* et remplacez le code du modèle par le code suivant, ce qui entraîne une base de données doit être créé si nécessaire et les données de test de charge dans la nouvelle base de données.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   Le `Seed` méthode prend l’objet de contexte de base de données comme paramètre d’entrée, et le code dans la méthode utilise cet objet pour ajouter de nouvelles entités à la base de données. Pour chaque type d’entité, le code crée une collection de nouvelles entités, les ajoute à approprié `DbSet` propriété, puis enregistre les modifications apportées à la base de données. Il n’est pas nécessaire d’appeler le `SaveChanges` méthode après chaque groupe d’entités, comme le fait ici, mais cela vous permet de localiser la source d’un problème si une exception se produit pendant que le code écrit pour la base de données.

2. Pour indiquer à Entity Framework à utiliser votre classe d’initialiseur, ajoutez un élément à la `entityFramework` élément dans l’application *Web.config* fichier (l’un dans le dossier racine du projet), comme indiqué dans l’exemple suivant :

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   Le `context type` Spécifie le nom de classe de contexte complet et l’assembly, il se trouve dans, et le `databaseinitializer type` Spécifie le nom qualifié complet de la classe de l’initialiseur et de l’assembly. (Lorsque vous ne souhaitez pas EF d’utiliser l’initialiseur, vous pouvez définir un attribut sur le `context` élément : `disableDatabaseInitialization="true"`.) Pour plus d’informations, consultez [fichier de configuration](/ef/ef6/fundamentals/configuring/config-file).

   Une alternative à la définition de l’initialiseur le *Web.config* fichier consiste à faire dans le code en ajoutant un `Database.SetInitializer` instruction à la `Application_Start` méthode dans le *Global.asax.cs* fichier. Pour plus d’informations, consultez [compréhension des initialiseurs de base de données dans Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

L’application est maintenant configurée afin que lorsque vous accédez à la base de données pour la première fois dans une exécution donnée de l’application, Entity Framework compare la base de données au modèle (votre `SchoolContext` et classes d’entité). S’il existe une différence, l’application supprime et recrée la base de données.

> [!NOTE]
> Lorsque vous déployez une application sur un serveur web de production, vous devez supprimer ou désactiver le code qui supprime et recrée la base de données. Vous pouvez utiliser dans un didacticiel plus loin dans cette série.

## <a name="set-up-ef-6-to-use-localdb"></a>Configuration d’EF 6 pour utiliser la base de données locale

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) est une version allégée du moteur de base de données SQL Server Express. Il est facile à installer et configurer démarre à la demande et s’exécute en mode utilisateur. Base de données locale s’exécute dans un mode spécial de l’exécution de SQL Server Express qui vous permet de travailler avec les bases de données en tant que *.mdf* fichiers. Vous pouvez placer les fichiers de base de données de base de données locale dans le *application\_données* dossier d’un projet web si vous souhaitez être en mesure de copier la base de données avec le projet. La fonctionnalité d’instance utilisateur dans SQL Server Express vous permet également de travailler avec *.mdf* fichiers, mais la fonctionnalité d’instance utilisateur est déconseillé ; par conséquent, il est recommandé LocalDB pour travailler avec *.mdf* fichiers. LocalDB est installé par défaut avec Visual Studio.

En règle générale, SQL Server Express n’est pas utilisé pour les applications web de production. Base de données locale en particulier est déconseillé pour la production avec une application web, car il n’est pas conçu pour fonctionner avec IIS.

- Dans ce didacticiel, vous allez utiliser avec la base de données locale. Ouvrez l’application *Web.config* fichier, puis ajoutez un `connectionStrings` élément précédant le `appSettings` élément, comme indiqué dans l’exemple suivant. (Assurez-vous que vous mettez à jour le *Web.config* fichier dans le dossier racine du projet. Il existe également un *Web.config* de fichiers dans le *vues* sous-dossier que vous n’avez pas besoin de mettre à jour.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

La chaîne de connexion que vous avez ajouté Spécifie que Entity Framework utilise une base de données de base de données locale nommée *ContosoUniversity1.mdf*. (La base de données n’existe pas encore, mais EF va la créer.) Si vous souhaitez créer la base de données dans votre *application\_données* dossier, vous pouvez ajouter `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` à la chaîne de connexion. Pour plus d’informations sur les chaînes de connexion, consultez [chaînes de connexion SQL Server pour les Applications Web ASP.NET](/previous-versions/aspnet/jj653752(v=vs.110)).

Vous n’avez pas besoin une chaîne de connexion dans le *Web.config* fichier. Si vous ne fournissez pas une chaîne de connexion, Entity Framework utilise une chaîne de connexion par défaut en fonction de votre classe de contexte. Pour plus d’informations, consultez [Code First pour une base de données](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Créer un contrôleur et des vues

Maintenant, vous allez créer une page web pour afficher des données. Le processus de demande les données automatiquement déclenche la création de la base de données. Vous commencerez par créer un nouveau contrôleur. Mais avant cela, générez le projet pour rendre les classes de modèle et au contexte disponible pour la structure de contrôleur MVC.

1. Avec le bouton droit le **contrôleurs** dossier **l’Explorateur de solutions**, sélectionnez **ajouter**, puis cliquez sur **nouvel élément structuré**.
2. Dans le **ajouter une structure** boîte de dialogue, sélectionnez **contrôleur MVC 5 avec vues, utilisant Entity Framework**, puis choisissez **ajouter**.

     ![Ajouter la boîte de dialogue Scaffold dans Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. Dans le **ajouter un contrôleur** boîte de dialogue, effectuez les sélections suivantes, puis choisissez **ajouter**:

   - Classe de modèle : **Student (ContosoUniversity.Models)**. (Si vous ne voyez pas cette option dans la liste déroulante, générez le projet et réessayez.)
   - Classe de contexte de données : **SchoolContext (ContosoUniversity.DAL)**.
   - Nom du contrôleur : **StudentController** (pas StudentsController).
   - Conservez les valeurs par défaut pour les autres champs.

     Lorsque vous cliquez sur **ajouter**, le Générateur de modèles automatique crée un *StudentController.cs* fichier et un ensemble de vues (*.cshtml* fichiers) qui fonctionnent avec le contrôleur. À l’avenir lorsque vous créez des projets qui utilisent Entity Framework, vous pouvez également tirer parti des fonctionnalités supplémentaires de la génération de modèles automatique : créer votre première classe de modèle, ne créez pas une chaîne de connexion, puis dans le **ajouter un contrôleur** boîte spécifier **nouveau contexte de données** en sélectionnant le **+** situé en regard **classe de contexte de données**. Le Générateur de modèles automatique crée votre `DbContext` votre connexion et la classe de chaîne, ainsi que le contrôleur et des vues.
4. Visual Studio ouvre le *Controllers\StudentController.cs* fichier. Vous voyez qu’une variable de classe a été créée qui instancie un objet de contexte de base de données :

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Le `Index` méthode d’action Obtient une liste d’étudiants à partir de la *étudiants* entité définie en lisant le `Students` propriété de l’instance de contexte de base de données :

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     Le *Student\Index.cshtml* affiche cette liste dans une table :

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Appuyez sur Ctrl + F5 pour exécuter le projet. (Si vous obtenez une erreur « Impossible de créer le cliché instantané », fermez le navigateur et réessayez.)

     Cliquez sur le **étudiants** onglet pour afficher les données de test qui le `Seed` méthode inséré. Selon l’étroitesse ce champ est la fenêtre du navigateur, vous verrez le lien de l’onglet étudiant dans la barre d’adresses principales, ou vous devrez cliquez sur le coin supérieur droit pour afficher le lien.

     ![Bouton de menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Afficher la base de données

Lorsque vous avez exécuté la page des étudiants et que l’application a essayé d’accéder à la base de données, EF découvert qu’il n’a aucune base de données et créé un. Ensuite, EF exécuté la méthode seed pour remplir la base de données.

Vous pouvez utiliser **Explorateur de serveurs** ou **Explorateur d’objets SQL Server** (SSOX) pour afficher la base de données dans Visual Studio. Pour ce didacticiel, vous allez utiliser **Explorateur de serveurs**.

1. Fermez le navigateur.
2. Dans **Explorateur de serveurs**, développez **des connexions de données** (vous devrez peut-être d’abord sélectionner le bouton Actualiser), développez **contexte School (ContosoUniversity)**, puis développez  **Tables** pour afficher les tables dans votre nouvelle base de données.

3. Avec le bouton droit le **étudiant** de table et cliquez sur **afficher les données de Table** pour afficher les colonnes qui ont été créés et les lignes qui ont été insérées dans la table.

4. Fermer le **Explorateur de serveurs** connexion.

Le *ContosoUniversity1.mdf* et *.ldf* les fichiers de base de données se trouvent dans le *% USERPROFILE%* dossier.

Car vous utilisez le `DropCreateDatabaseIfModelChanges` initialiseur, vous pouvez maintenant apporter une modification apportée à la `Student` classe, exécutez à nouveau l’application, et la base de données serait automatiquement recréée conformément à vos modifications. Par exemple, si vous ajoutez un `EmailAddress` propriété le `Student` classe, réexécutez la page des étudiants, et examinez la table à nouveau, vous verrez un nouveau `EmailAddress` colonne.

## <a name="conventions"></a>Conventions

La quantité de code que vous deviez écrire dans l’ordre pour Entity Framework pouvoir créer une base de données complète pour vous est minimale en raison de *conventions*, ou hypothèses qui rend d’Entity Framework. Certains d'entre eux ont déjà été observées ou ont été utilisées sans votre connaître les :

- Les formulaires pluralisées des noms de classes d’entité sont utilisés comme noms de tables.
- Les noms de propriété d’entité sont utilisées pour les noms de colonne.
- Propriétés de l’entité qui sont nommées `ID` ou *classname* `ID` sont reconnus comme des propriétés de clé primaire.
- Une propriété est interprétée comme une propriété de clé étrangère si elle se nomme *&lt;nom de propriété de navigation&gt;&lt;nom de propriété de clé primaire&gt;* (par exemple, `StudentID` pour la `Student` propriété de navigation depuis le `Student` clé primaire de l’entité est `ID`). Propriétés de clé étrangère peuvent également être nommées les mêmes simplement &lt;nom de propriété de clé primaire&gt; (par exemple, `EnrollmentID` dans la mesure où le `Enrollment` clé primaire de l’entité est `EnrollmentID`).

Vous avez vu que les conventions peuvent être substituées. Par exemple, vous avez spécifié que les noms de table ne doit pas être mis au pluriel, et vous verrez plus tard comment marquer explicitement une propriété comme une propriété de clé étrangère.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations sur EF 6, consultez ces articles :

* [Accès aux données ASP.NET - Ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Conventions Code First](/ef/ef6/modeling/code-first/conventions/built-in)

* [Création d’un modèle de données plus complexe](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créé une application web MVC
> * Configurer le style du site
> * Installé Entity Framework 6
> * Modèle de données créé
> * Contexte de base de données créé
> * Base de données initialisée avec des données de test
> * Configuration d’EF 6 pour utiliser la base de données locale
> * Un contrôleur et des vues créés
> * Base de données affichée

Passez à l’article suivant pour apprendre à examiner et personnaliser la créer, lire, mettre à jour, supprimer (CRUD) les code dans vos contrôleurs et les vues.
> [!div class="nextstepaction"]
> [Implémenter la fonctionnalité CRUD de base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)