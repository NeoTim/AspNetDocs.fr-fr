---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Bien démarrer avec Entity Framework 4.0 Database First et 4 d’ASP.NET Web Forms - partie 7 | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide d’Entity Framework. L’exemple d’application est en cours...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 48092838ac0b00137ae0a4e37391c31883afcd09
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57027916"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Bien démarrer avec Entity Framework 4.0 Database First et 4 les Web Forms ASP.NET - partie 7
====================
par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide de l’Entity Framework 4.0 et Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Utilisation des procédures stockées

Dans le didacticiel précédent, vous avez implémenté un modèle d’héritage de table par hiérarchie. Ce didacticiel vous explique comment utiliser des procédures stockées pour mieux contrôler les accès de base de données.

Entity Framework vous permet de spécifier qu’il doit utiliser des procédures stockées pour l’accès de base de données. Pour n’importe quel type d’entité, vous pouvez spécifier une procédure stockée à utiliser pour la création, la mise à jour ou supprimant des entités de ce type. Puis dans le modèle de données vous pouvez ajouter des références à des procédures stockées que vous pouvez utiliser pour effectuer des tâches telles que la récupération des jeux d’entités.

À l’aide de procédures stockées est une exigence courante pour l’accès de base de données. Dans certains cas, un administrateur de base de données peut nécessiter que tous les accès de base de données passent par des procédures stockées pour des raisons de sécurité. Dans d’autres cas, vous souhaiterez intégrer la logique métier certains processus qui Entity Framework utilise lorsqu’il met à jour la base de données. Par exemple, chaque fois qu’une entité est supprimée, vous souhaiterez copier dans une base de données d’archive. Ou chaque fois qu’une ligne est mise à jour, vous souhaiterez écrire une ligne dans une table de journalisation qui enregistre qui a apporté la modification. Vous pouvez effectuer ces types de tâches dans une procédure stockée qui est appelée chaque fois qu’Entity Framework supprime une entité ou met à jour une entité.

Comme dans le didacticiel précédent, vous créerez pas toutes les nouvelles pages. Au lieu de cela, vous allez modifier la façon d’Entity Framework accède à la base de données pour certaines des pages que vous avez déjà créé.

Dans ce didacticiel, vous allez créer des procédures stockées dans la base de données pour l’insertion de `Student` et `Instructor` entités. Vous allez les ajouter au modèle de données, et vous allez spécifier que Entity Framework doit les utiliser pour l’ajout de `Student` et `Instructor` entités à la base de données. Vous allez également créer une procédure stockée que vous pouvez utiliser pour récupérer `Course` entités.

## <a name="creating-stored-procedures-in-the-database"></a>Création de procédures stockées dans la base de données

(Si vous utilisez le *School.mdf* fichier à partir du projet est disponible en téléchargement avec ce didacticiel, vous pouvez ignorer cette section, car les procédures stockées existent déjà.)

Dans **Explorateur de serveurs**, développez *School.mdf*, avec le bouton droit **Stored Procedures**, puis sélectionnez **ajouter une nouvelle procédure stockée**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copiez les instructions SQL suivantes et collez-les dans la fenêtre de la procédure stockée, en remplaçant la procédure stockée squelette.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` entités ont quatre propriétés : `PersonID`, `LastName`, `FirstName`, et `EnrollmentDate`. La base de données génère la valeur d’ID automatiquement, et la procédure stockée accepte des paramètres pour les trois autres. La procédure stockée retourne la valeur de la clé d’enregistrement de la nouvelle ligne afin que l’Entity Framework peut effectuer le suivi de qui dans la version de l’entité qu’il conserve en mémoire.

Enregistrez et fermez la fenêtre de la procédure stockée.

Créer un `InsertInstructor` procédure stockée de la même manière, à l’aide d’instructions SQL suivantes :

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Créer `Update` des procédures stockées pour `Student` et `Instructor` entités également. (La base de données a déjà un `DeletePerson` procédure stockée qui fonctionnera pour les deux `Instructor` et `Student` entités.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

Dans ce didacticiel, vous allez mapper tous les trois fonctions : insert, update et delete--pour chaque type d’entité. Entity Framework version 4, vous pouvez associer qu’un ou deux de ces fonctions à des procédures stockées sans mappage d’autres, à une exception près : Si vous mappez la fonction de mise à jour, mais pas la fonction de suppression, Entity Framework lève une exception lorsque vous Essayez de supprimer une entité. Dans Entity Framework version 3.5, vous n’avez pas de flexibilité dans le mappage de procédures stockées : Si vous avez mappé une seule fonction vous deviez mapper tous les trois.

Pour créer une procédure stockée qui indique plutôt que de mises à jour des données, créez-le qui sélectionne tous les `Course` entités, à l’aide d’instructions SQL suivantes :

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Ajout des procédures stockées au modèle de données

Les procédures stockées sont maintenant définies dans la base de données, mais ils doivent être ajoutés au modèle de données pour les rendre disponibles pour Entity Framework. Ouvrez *SchoolModel.edmx*, cliquez sur l’aire de conception, puis sélectionnez **modèle de mise à jour à partir de la base de données**. Dans le **ajouter** onglet de la **choisir vos objets de base de données** boîte de dialogue, développez **Stored Procedures**, sélectionnez les procédures stockées qui vient d’être créés et le `DeletePerson` procédure stockée, puis cliquez sur **Terminer**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Les procédures stockées de mappage

Dans le Concepteur de modèle de données, cliquez sur le `Student` entité, puis sélectionnez **mappage de procédure stockée**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Le **détails de Mapping** fenêtre s’affiche, dans laquelle vous pouvez spécifier des procédures stockées que Entity Framework doit utiliser pour l’insertion, la mise à jour et suppression d’entités de ce type.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Définir le **insérer** à fonction **InsertStudent**. La fenêtre affiche une liste de paramètres de procédure stockée, chacun d’eux doit être mappée à une propriété d’entité. Deux d'entre elles sont mappées automatiquement, car les noms sont identiques. Il n’existe aucune propriété d’entité nommée `FirstName`, de sorte que vous devez sélectionner manuellement `FirstMidName` à partir d’une liste déroulante qui affiche les propriétés de l’entité disponible. (Il s’agit, car vous avez modifié le nom de la `FirstName` propriété `FirstMidName` dans le premier didacticiel.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Dans le même **détails de Mapping** fenêtre, mappez le `Update` fonctionner à la `UpdateStudent` procédure stockée (veillez à spécifier `FirstMidName` comme valeur du paramètre `FirstName`, comme vous l’avez fait le `Insert` procédure stockée) et le `Delete` à fonction le `DeletePerson` procédure stockée.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Suivez la même procédure pour mapper l’insertion, mise à jour et suppression des procédures stockées pour les formateurs à la `Instructor` entité.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Pour les procédures stockées qui lisent et non pas les données de mise à jour, vous utilisez le **Explorateur de modèles** retourne tapez-la fenêtre pour mapper la procédure stockée à l’entité. Dans le Concepteur de modèle de données, cliquez sur l’aire de conception et sélectionnez **Explorateur de modèles**. Ouvrez le **SchoolModel.Store** nœud, puis ouvrez le **Stored Procedures** nœud. Cliquez sur le `GetCourses` procédure stockée et sélectionnez **ajouter une importation de fonction**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

Dans le **ajouter une importation de fonction** boîte de dialogue **retourne une Collection de** sélectionnez **entités**, puis sélectionnez `Course` comme le type d’entité retourné. Lorsque vous avez terminé, cliquez sur **OK**. Enregistrez et fermez le *.edmx* fichier.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>À l’aide d’insérer, mettre à jour et supprimer des procédures stockées

Les procédures stockées pour insérer, mettre à jour et supprimer des données sont utilisées par Entity Framework automatiquement une fois que vous avez ajoutés au modèle de données et les mappés aux entités appropriées. Vous pouvez maintenant exécuter le *StudentsAdd.aspx* page, et chaque fois que vous créez un nouvel étudiant, Entity Framework utilisera le `InsertStudent` procédure stockée pour ajouter la nouvelle ligne à la `Student` table.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Exécutez le *Students.aspx* page et le nouvel étudiant apparaît dans la liste.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Modifier le nom pour vérifier que la fonction de mise à jour fonctionne, puis supprimez l’étudiant pour vérifier que la fonction de suppression fonctionne.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>À l’aide de procédures stockées Select

Entity Framework n’exécute pas automatiquement les procédures stockées comme `GetCourses`, et vous ne pouvez pas les utiliser avec le `EntityDataSource` contrôle. Pour ce faire, vous les appelez à partir de code.

Ouvrez le *InstructorsCourses.aspx.cs* fichier. Le `PopulateDropDownLists` méthode utilise une requête LINQ-to-Entities pour récupérer toutes les entités course afin qu’il peut parcourir la liste et déterminer celles qui un formateur est affecté et celles qui est non affectés :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Remplacez ceci par le code suivant :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

La page utilise désormais le `GetCourses` procédure stockée pour récupérer la liste de tous les cours. Exécutez la page pour vérifier qu’elle fonctionne comme avant.

(Propriétés de navigation d’entités récupérées par une procédure stockée ne peuvent pas être remplies automatiquement avec les données liées à ces entités selon `ObjectContext` paramètres par défaut. Pour plus d’informations, consultez [le chargement des objets connexes](https://msdn.microsoft.com/library/bb896272.aspx) dans MSDN Library.)

Dans le didacticiel suivant, vous allez apprendre à utiliser les fonctionnalités Dynamic Data pour le rendre plus facile de programme et test règles de mise en forme et validation de données. Au lieu de spécifier chaque page web et des règles telles que des chaînes de format de données ou non un champ est obligatoire, vous pouvez spécifier des règles de ce type dans les métadonnées de modèle de données et elles sont appliquées automatiquement sur chaque page.

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-8.md)
