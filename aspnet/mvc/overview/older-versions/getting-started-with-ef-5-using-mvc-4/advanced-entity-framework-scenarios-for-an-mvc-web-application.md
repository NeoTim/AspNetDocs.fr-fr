---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Scénarios de Entity Framework avancés pour une application Web MVC (10 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: f8f079f6d8ea663c6888456be422a2bae93a4b87
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "86163469"
---
# <a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Scénarios de Entity Framework avancés pour une application Web MVC (10 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels depuis le début ou [Télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et démarrer ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [Téléchargez le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire le problème. Vous pouvez généralement trouver la solution au problème en comparant votre code au code terminé. Pour obtenir des erreurs courantes et comment les résoudre, consultez [Erreurs et solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Dans le didacticiel précédent, vous avez implémenté le référentiel et les modèles d’unité de travail. Ce didacticiel aborde les sujets suivants :

- Exécution de requêtes SQL brutes.
- Exécution de requêtes sans suivi.
- Examen des requêtes envoyées à la base de données.
- Utilisation des classes proxy.
- Désactivation de la détection automatique des modifications.
- Désactivation de la validation lors de l’enregistrement des modifications.
- [Erreurs et contournement](#errors)

Pour la plupart de ces rubriques, vous utiliserez les pages que vous avez déjà créées. Pour utiliser des données SQL brutes pour effectuer des mises à jour en bloc, vous allez créer une nouvelle page qui met à jour le nombre de crédits de tous les cours dans la base de données :

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Et pour utiliser une requête de non-suivi, vous allez ajouter une nouvelle logique de validation à la page de modification du département :

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Exécution de requêtes SQL brutes

L’API Entity Framework Code First comprend des méthodes qui vous permettent de transmettre des commandes SQL directement à la base de données. Les options suivantes s’offrent à vous :

- Utilisez la méthode `DbSet.SqlQuery` pour les requêtes qui renvoient des types d’entités. Les objets retournés doivent être du type attendu par l' `DbSet` objet, et ils sont suivis automatiquement par le contexte de base de données, sauf si vous désactivez le suivi. (Consultez la section suivante sur la `AsNoTracking` méthode.)
- Utilisez la `Database.SqlQuery` méthode pour les requêtes qui retournent des types qui ne sont pas des entités. Les données renvoyées ne font pas l’objet d’un suivi par le contexte de base de données, même si vous utilisez cette méthode pour récupérer des types d’entités.
- Utilisez la [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) pour les commandes qui ne sont pas des requêtes.

L’un des avantages d’utiliser Entity Framework est que cela évite de lier votre code trop étroitement à une méthode particulière de stockage des données. Il le fait en générant des requêtes et des commandes SQL pour vous, ce qui vous évite d’avoir à les écrire vous-même. Toutefois, il existe des scénarios exceptionnels lorsque vous devez exécuter des requêtes SQL spécifiques que vous avez créées manuellement, et ces méthodes vous permettent de gérer ces exceptions.

Comme c’est toujours le cas lorsque vous exécutez des commandes SQL dans une application web, vous devez prendre des précautions pour protéger votre site contre des attaques par injection de code SQL. Une manière de procéder consiste à utiliser des requêtes paramétrables pour vous assurer que les chaînes soumises par une page web ne peuvent pas être interprétées comme des commandes SQL. Dans ce didacticiel, vous utiliserez des requêtes paramétrables lors de l’intégration de l’entrée utilisateur dans une requête.

### <a name="calling-a-query-that-returns-entities"></a>Appel d’une requête qui retourne des entités

Supposons que vous souhaitiez que la `GenericRepository` classe fournisse une souplesse de filtrage et de tri supplémentaire sans que vous ayez besoin de créer une classe dérivée avec des méthodes supplémentaires. Une façon d’y parvenir consiste à ajouter une méthode qui accepte une requête SQL. Vous pouvez ensuite spécifier tout type de filtrage ou de tri souhaité dans le contrôleur, par exemple une `Where` clause qui dépend d’une jointure ou d’une sous-requête. Dans cette section, vous allez apprendre à implémenter une telle méthode.

Créez la `GetWithRawSql` méthode en ajoutant le code suivant à *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

Dans *CourseController.cs*, appelez la nouvelle méthode à partir de la `Details` méthode, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Dans ce cas, vous pourriez avoir utilisé la `GetByID` méthode, mais vous utilisez la `GetWithRawSql` méthode pour vérifier que la `GetWithRawSQL` méthode fonctionne.

Exécutez la page de détails pour vérifier que la requête SELECT fonctionne (sélectionnez l’onglet **course** , puis les **Détails** d’un cours).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Appel d’une requête qui retourne d’autres types d’objets

Précédemment, vous avez créé une grille de statistiques des étudiants pour la page About, qui montrait le nombre d’étudiants pour chaque date d’inscription. Le code qui le fait dans *HomeController.cs* utilise LINQ :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Supposons que vous souhaitiez écrire le code qui récupère ces données directement dans SQL au lieu d’utiliser LINQ. Pour ce faire, vous devez exécuter une requête qui retourne autre chose que des objets d’entité, ce qui signifie que vous devez utiliser la `Database.SqlQuery` méthode.

Dans *HomeController.cs*, remplacez l’instruction LINQ dans la `About` méthode par le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Exécutez la page about. Elle affiche les mêmes données qu’auparavant.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Appel d’une requête Update

Supposons que les administrateurs de Contoso University souhaitent pouvoir effectuer des modifications en bloc dans la base de données, telles que la modification du nombre de crédits pour chaque cours. Si l’université a un grand nombre de cours, il serait inefficace de les récupérer tous sous forme d’entités et de les modifier individuellement. Dans cette section, vous allez implémenter une page Web qui permet à l’utilisateur de spécifier un facteur par lequel modifier le nombre de crédits pour tous les cours, et vous apprendrez la modification en exécutant une `UPDATE` instruction SQL. La page web ressemblera à l’illustration suivante :

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Dans le didacticiel précédent, vous avez utilisé le référentiel générique pour lire et mettre à jour des `Course` entités dans le `Course` contrôleur. Pour cette opération de mise à jour en bloc, vous devez créer une nouvelle méthode de référentiel qui ne se trouve pas dans le référentiel générique. Pour ce faire, vous allez créer une `CourseRepository` classe dédiée qui dérive de la `GenericRepository` classe.

Dans le dossier *dal* , créez *CourseRepository.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

Dans *UnitOfWork.cs*, remplacez le `Course` type de dépôt `GenericRepository<Course>` par`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

Dans *CourseController.cs*, ajoutez une `UpdateCourseCredits` méthode :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Cette méthode sera utilisée pour `HttpGet` et `HttpPost` . Lorsque la `HttpGet` `UpdateCourseCredits` méthode s’exécute, la `multiplier` variable est null et la vue affiche une zone de texte vide et un bouton envoyer, comme indiqué dans l’illustration précédente.

Lorsque l’utilisateur clique sur le bouton **mettre à jour** et que la `HttpPost` méthode s’exécute, `multiplier` la valeur est entrée dans la zone de texte. Le code appelle ensuite la `UpdateCourseCredits` méthode de dépôt, qui retourne le nombre de lignes affectées, et cette valeur est stockée dans l' `ViewBag` objet. Lorsque la vue reçoit le nombre de lignes affectées dans l' `ViewBag` objet, elle affiche ce nombre au lieu de la zone de texte et du bouton envoyer, comme indiqué dans l’illustration suivante :

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Créez un affichage dans le dossier *Views\Course* pour la page mettre à jour les crédits de cours :

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

Dans *Views\Course\UpdateCourseCredits.cshtml*, remplacez le code du modèle par le code suivant :

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Exécutez la méthode `UpdateCourseCredits` en sélectionnant l’onglet **Courses**, puis en ajoutant « /UpdateCourseCredits » à la fin de l’URL dans la barre d’adresse du navigateur (par exemple : `http://localhost:50205/Course/UpdateCourseCredits`). Entrez un nombre dans la zone de texte :

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Cliquez sur **Update**. Vous voyez le nombre de lignes affectées :

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Cliquez sur **Revenir à la liste** pour afficher la liste des cours avec le nombre révisé de crédits.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Pour plus d’informations sur les requêtes SQL brutes, consultez [requêtes SQL brutes](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) sur le blog de l’équipe Entity Framework.

## <a name="no-tracking-queries"></a>sans suivi

Lorsqu’un contexte de base de données récupère des lignes de base de données et crée des objets d’entité qui les représentent, il effectue par défaut le suivi du fait que les entités en mémoire sont synchronisées avec ce qui se trouve dans la base de données. Les données en mémoire agissent comme un cache et sont utilisées quand vous mettez à jour une entité. Cette mise en cache est souvent inutile dans une application web, car les instances de contexte ont généralement une durée de vie courte (une instance est créée puis supprimée pour chaque requête) et le contexte qui lit une entité est généralement supprimé avant que cette entité soit réutilisée.

Vous pouvez spécifier si le contexte effectue le suivi des objets d’entité pour une requête à l’aide de la `AsNoTracking` méthode. Voici des scénarios classiques où vous voulez procéder ainsi :

- La requête récupère un volume important de données qui désactive le suivi, ce qui peut améliorer considérablement les performances.
- Vous souhaitez attacher une entité afin de la mettre à jour, mais vous avez précédemment récupéré la même entité dans un but différent. Comme l’entité est déjà suivie par le contexte de base de données, vous ne pouvez pas attacher l’entité que vous voulez modifier. Une façon d’éviter ce problème consiste à utiliser l' `AsNoTracking` option avec la requête précédente.

Dans cette section, vous allez implémenter une logique métier qui illustre le deuxième scénario. En particulier, vous allez appliquer une règle d’entreprise qui indique qu’un formateur ne peut pas être l’administrateur de plus d’un service.

Dans *DepartmentController.cs*, ajoutez une nouvelle méthode que vous pouvez appeler à partir `Edit` des `Create` méthodes et pour vous assurer qu’aucun des deux services n’a le même administrateur :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Ajoutez du code dans le `try` bloc de la `HttpPost` `Edit` méthode pour appeler cette nouvelle méthode s’il n’y a aucune erreur de validation. Le `try` bloc ressemble maintenant à l’exemple suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Exécutez la page de modification de département et essayez de changer l’administrateur d’un service en un formateur qui est déjà l’administrateur d’un autre service. Vous recevez le message d’erreur attendu :

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Exécutez à présent la page de modification du département et cette fois, modifiez le montant du **budget** . Lorsque vous cliquez sur **Enregistrer**, une page d’erreur s’affiche :

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Le message d’erreur d’exception est «» s’est `An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.` produit en raison de la séquence d’événements suivante :

- La `Edit` méthode appelle la `ValidateOneAdministratorAssignmentPerInstructor` méthode, qui récupère tous les services qui ont Kim Abercrombie comme administrateur. Cela entraîne la lecture du département anglais. Étant donné qu’il s’agit du département en cours de modification, aucune erreur n’est signalée. À la suite de cette opération de lecture, toutefois, l’entité de service en anglais qui a été lue à partir de la base de données est maintenant suivie par le contexte de base de données.
- La `Edit` méthode tente de définir l' `Modified` indicateur sur l’entité de service en anglais créée par le Binder de modèle MVC, mais cela échoue car le contexte est déjà suivi d’une entité pour le département anglais.

Une solution à ce problème consiste à conserver le contexte du suivi des entités de service en mémoire récupérées par la requête de validation. Il n’y a aucun inconvénient à effectuer cette opération, car vous ne pourrez pas mettre à jour cette entité ou la lire de nouveau de manière à tirer parti de sa mise en cache en mémoire.

Dans *DepartmentController.cs*, dans la `ValidateOneAdministratorAssignmentPerInstructor` méthode, spécifiez aucun suivi, comme illustré ci-dessous :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Répétez votre tentative de modification du montant du **budget** d’un service. Cette fois-ci, l’opération réussit et le site renvoie comme prévu à la page d’index des départements, en présentant la valeur de budget révisée.

## <a name="examining-queries-sent-to-the-database"></a>Examen des requêtes envoyées à la base de données

Il est parfois utile de pouvoir voir les requêtes SQL réelles qui sont envoyées à la base de données. Pour ce faire, vous pouvez examiner une variable de requête dans le débogueur ou appeler la méthode de la requête `ToString` . Pour essayer cela, vous allez examiner une requête simple, puis examiner ce qui se passe au fur et à mesure que vous ajoutez des options telles que le chargement, le filtrage et le tri hâtif.

Dans *Controllers/CourseController*, remplacez la `Index` méthode par le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

À présent, définissez un point d’arrêt dans *GenericRepository.cs* sur les `return query.ToList();` `return orderBy(query).ToList();` instructions et de la `Get` méthode. Exécutez le projet en mode débogage et sélectionnez la page index du cours. Lorsque le code atteint le point d’arrêt, examinez la `query` variable. Vous voyez la requête qui est envoyée à SQL Server. Il s’agit d’une simple `Select` instruction :

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Les requêtes peuvent être trop longues pour s’afficher dans les fenêtres de débogage dans Visual Studio. Pour afficher l’intégralité de la requête, vous pouvez copier la valeur de la variable et la coller dans un éditeur de texte :

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Vous allez maintenant ajouter une liste déroulante à la page d’index des cours afin que les utilisateurs puissent filtrer pour un service particulier. Vous allez trier les cours par titre et spécifier le chargement hâtif pour la propriété de `Department` navigation. Dans *CourseController.cs*, remplacez la `Index` méthode par le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

La méthode reçoit la valeur sélectionnée de la liste déroulante dans le `SelectedDepartment` paramètre. Si rien n’est sélectionné, ce paramètre a la valeur null.

Une `SelectList` collection contenant tous les services est passée à la vue pour la liste déroulante. Les paramètres passés au `SelectList` constructeur spécifient le nom du champ de valeur, le nom du champ de texte et l’élément sélectionné.

Pour la `Get` méthode du `Course` référentiel, le code spécifie une expression de filtre, un ordre de tri et un chargement hâtif pour la `Department` propriété de navigation. L’expression de filtre retourne toujours `true` si rien n’est sélectionné dans la liste déroulante (autrement dit, `SelectedDepartment` est null).

Dans *Views\Course\Index.cshtml*, juste avant la `table` balise d’ouverture, ajoutez le code suivant pour créer la liste déroulante et un bouton Envoyer :

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Les points d’arrêt étant toujours définis dans la `GenericRepository` classe, exécutez la page index des cours. Passez aux deux premières fois que le code atteint un point d’arrêt, afin que la page s’affiche dans le navigateur. Sélectionnez un service dans la liste déroulante, puis cliquez sur **Filtrer**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Cette fois, le premier point d’arrêt est destiné à la requête Departments pour la liste déroulante. Ignorez cette valeur et affichez la `query` variable la prochaine fois que le code atteint le point d’arrêt afin de voir à quoi `Course` ressemble la requête. Vous verrez un résultat similaire à ce qui suit :

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Vous pouvez voir que la requête est désormais une `JOIN` requête qui charge `Department` des données avec les `Course` données et qu’elle comprend une `WHERE` clause.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Utilisation des classes proxy

Lorsque le Entity Framework crée des instances d’entité (par exemple, lorsque vous exécutez une requête), il les crée souvent comme instances d’un type dérivé généré de manière dynamique qui agit comme un proxy pour l’entité. Ce proxy remplace certaines propriétés virtuelles de l’entité pour insérer des raccordements pour l’exécution automatique des actions lors de l’accès à la propriété. Par exemple, ce mécanisme est utilisé pour prendre en charge le chargement différé des relations.

La plupart du temps, vous n’avez pas besoin d’être conscient de cette utilisation des proxys, mais il existe des exceptions :

- Dans certains scénarios, vous souhaiterez peut-être empêcher le Entity Framework de créer des instances de proxy. Par exemple, la sérialisation des instances non proxy peut être plus efficace que la sérialisation des instances de proxy.
- Lorsque vous instanciez une classe d’entité à l’aide de l' `new` opérateur, vous n’avez pas d’instance de proxy. Cela signifie que vous ne bénéficiez pas de fonctionnalités telles que le chargement différé et le suivi automatique des modifications. C’est généralement parfait. en général, vous n’avez pas besoin de chargement différé, car vous créez une nouvelle entité qui n’est pas dans la base de données, et vous n’avez généralement pas besoin de suivi des modifications si vous marquez explicitement l’entité comme `Added` . Toutefois, si vous avez besoin d’un chargement différé et que vous avez besoin d’un suivi des modifications, vous pouvez créer de nouvelles instances d’entité avec des proxies à l’aide `Create` de la méthode de la `DbSet` classe.
- Vous pouvez obtenir un type d’entité réel à partir d’un type de proxy. Vous pouvez utiliser la `GetObjectType` méthode de la `ObjectContext` classe pour obtenir le type d’entité réel d’une instance de type de proxy.

Pour plus d’informations, consultez [utilisation des proxies](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) sur le blog de l’équipe Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Désactivation de la détection automatique des modifications

Entity Framework détermine la manière dont une entité a changé (et par conséquent les mises à jour qui doivent être envoyées à la base de données) en comparant les valeurs en cours d’une entité avec les valeurs d’origine. Les valeurs d’origine sont stockées lors de l’interrogation ou de l’attachement de l’entité. Certaines des méthodes qui provoquent la détection automatique des modifications sont les suivantes :

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si vous effectuez le suivi d’un grand nombre d’entités et que vous appelez une de ces méthodes plusieurs fois dans une boucle, vous pouvez obtenir des améliorations significatives en matière de performances en désactivant temporairement la détection automatique des modifications à l’aide de la propriété [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) . Pour plus d’informations, consultez [détection automatique des modifications](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Désactivation de la validation lors de l’enregistrement des modifications

Lorsque vous appelez la `SaveChanges` méthode, par défaut, la Entity Framework valide les données de toutes les propriétés de toutes les entités modifiées avant de mettre à jour la base de données. Si vous avez mis à jour un grand nombre d’entités et que vous avez déjà validé les données, ce travail n’est pas nécessaire et vous pouvez faire en sorte que le processus d’enregistrement des modifications prenne moins de temps en désactivant temporairement la validation. Vous pouvez le faire à l’aide de la propriété [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) . Pour plus d’informations, consultez [validation](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Récapitulatif

Cette série de didacticiels s’achève sur l’utilisation de la Entity Framework dans une application MVC ASP.NET. Vous trouverez des liens vers d’autres ressources de Entity Framework dans le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

Pour plus d’informations sur la façon de déployer votre application Web une fois que vous l’avez créée, consultez [mappage de contenu de déploiement ASP.net](https://msdn.microsoft.com/library/bb386521.aspx) dans MSDN Library.

Pour plus d’informations sur les autres rubriques liées à MVC, telles que l’authentification et l’autorisation, consultez les [ressources recommandées pour MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Remerciements

- Tom Dykstra a écrit la version d’origine de ce didacticiel et est un rédactrice de programmation Senior sur l’équipe de contenu Microsoft Web Platform and Tools.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) a co-écrit ce didacticiel et a effectué la plus grande partie du travail de mise à jour pour EF 5 et MVC 4. Rick est un auteur de programmation senior pour Microsoft axé sur Azure et MVC.
- [Rowan Miller](http://www.romiller.com) et les autres membres de l’équipe Entity Framework assistent à la révision du code et ont aidé à déboguer de nombreux problèmes liés aux migrations qui se sont produits lors de la mise à jour du didacticiel pour EF 5.

## <a name="vb"></a>VB

Lorsque le didacticiel a été initialement créé, nous avons fourni des versions C# et VB du projet de téléchargement terminé. Avec cette mise à jour, nous fournissons un projet téléchargeable en C# pour chaque chapitre afin de faciliter la prise en main n’importe où dans la série, mais en raison des limitations de temps et d’autres priorités que nous n’avons pas effectuées pour VB. Si vous générez un projet VB à l’aide de ces didacticiels et que vous souhaitez le partager avec d’autres personnes, faites-le nous savoir.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Erreurs et solutions de contournement

### <a name="cannot-createshadow-copy"></a>Impossible de créer un cliché instantané

Message d’erreur :

*Impossible de créer le cliché instantané’DotNetOpenAuth. OpenId’lorsque ce fichier existe déjà.*

Solution :

Patientez quelques secondes, puis actualisez la page.

### <a name="update-database-not-recognized"></a>Mise à jour-base de données non reconnue

Message d’erreur :

*Le terme « Update-Database » n’est pas reconnu comme le nom d’une applet de commande, d’une fonction, d’un fichier de script ou d’un programme exécutable. Vérifiez l’orthographe du nom, ou si un chemin d’accès a été inclus, vérifiez que le chemin d’accès est correct, puis réessayez.* (À partir de la *`Update-Database`* commande dans le PMC.)

Solution :

Quittez Visual Studio. Rouvrez le projet et réessayez.

### <a name="validation-failed"></a>Échec de validation

Message d’erreur :

*La validation a échoué pour une ou plusieurs entités. Pour plus d’informations, consultez la propriété « EntityValidationErrors ».* (À partir de la *`Update-Database`* commande dans le PMC.)

Solution :

Ce problème peut être dû à des erreurs de validation lors de l’exécution de la `Seed` méthode. Pour obtenir des conseils sur le débogage de la méthode, consultez bases de l' [amorçage et débogage Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) `Seed` .

### <a name="http-50019-error"></a>Erreur HTTP 500,19

Message d’erreur :

*Erreur HTTP 500,19-erreur de serveur interne. impossible d’accéder à la page demandée, car les données de configuration associées à la page ne sont pas valides.*

Solution :

L’une des façons d’obtenir cette erreur est d’avoir plusieurs copies de la solution, chacune utilisant le même numéro de port. Vous pouvez généralement résoudre ce problème en quittant toutes les instances de Visual Studio, puis en redémarrant le projet sur lequel vous travaillez. Si cela ne fonctionne pas, essayez de modifier le numéro de port. Cliquez avec le bouton droit sur le fichier projet, puis cliquez sur Propriétés. Sélectionnez l’onglet **Web** , puis modifiez le numéro de port dans la zone de texte **URL du projet** .

### <a name="error-locating-sql-server-instance"></a>Erreur lors de la localisation de l’instance SQL Server

Message d’erreur :

*Une erreur liée au réseau ou spécifique à l’instance s’est produite lors de l’établissement d’une connexion à SQL Server. Le serveur est introuvable ou n’est pas accessible. Vérifiez que le nom de l’instance est correct et que SQL Server est configuré pour autoriser les connexions à distance. (fournisseur : interfaces réseau SQL, erreur : 26-erreur lors de la localisation du serveur/de l’instance spécifiés)*

Solution :

Vérifiez la chaîne de connexion. Si vous avez supprimé manuellement la base de données, modifiez le nom de la base de données dans la chaîne de construction.

> [!div class="step-by-step"]
> [Précédent](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md) 
>  [Suivant](building-the-ef5-mvc4-chapter-downloads.md)
