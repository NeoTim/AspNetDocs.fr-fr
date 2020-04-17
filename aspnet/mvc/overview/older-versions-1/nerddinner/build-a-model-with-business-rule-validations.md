---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Construire un modèle avec des validations de règles d’affaires ( Microsoft Docs
author: rick-anderson
description: L’étape 3 montre comment créer un modèle que nous pouvons utiliser à la fois pour interroger et mettre à jour la base de données de notre application NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 1a316e9051cf56cd4f1546336b334ace991c05b3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541635"
---
# <a name="build-a-model-with-business-rule-validations"></a>Créer un modèle avec des validations de règles d’entreprise

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 3 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 3 montre comment créer un modèle que nous pouvons utiliser à la fois pour interroger et mettre à jour la base de données de notre application NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner Étape 3: Construire le modèle

Dans un cadre de contrôleur de vue modèle, le terme « modèle » désigne les objets qui représentent les données de l’application, ainsi que la logique de domaine correspondante qui intègre la validation et les règles métier avec elle. Le modèle est à bien des égards le «cœur» d’une application basée sur MVC, et comme nous le verrons plus tard conduit fondamentalement le comportement de celui-ci.

Le cadre MVC ASP.NET prend en charge l’utilisation de toute technologie d’accès aux données, et les développeurs peuvent choisir parmi une variété d’options de données .NET riches pour mettre en œuvre leurs modèles, y compris: LINQ à des entités, LINQ à SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM, ou tout simplement brut ADO.NET DataReaders ou DataSets.

Pour notre application NerdDinner, nous allons utiliser LINQ à SQL pour créer un modèle simple qui correspond assez étroitement à notre conception de base de données, et ajoute une certaine logique de validation personnalisée et les règles d’affaires. Nous mettions ensuite en œuvre une classe de référentiel qui aide à réduire la mise en œuvre de la persistance des données à partir du reste de l’application, et nous permet de le tester facilement unitaire.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ à SQL est un ORM (art relationnel objet) qui expédie dans le cadre de .NET 3.5.

LINQ à SQL fournit un moyen facile de cartographier les tables de base de données pour les classes .NET que nous pouvons coder contre. Pour notre application NerdDinner, nous l’utiliserons pour cartographier les tables Dîners et RSVP dans notre base de données aux classes Dîner et RSVP. Les colonnes des tables Dîners et RSVP correspondent aux propriétés des cours de dîner et de RSVP. Chaque objet Dîner et RSVP représentera une rangée distincte dans les tables dîners ou RSVP dans la base de données.

LINQ à SQL nous permet d’éviter d’avoir à construire manuellement des relevés SQL pour récupérer et mettre à jour les objets Dinner et RSVP avec des données de base de données. Au lieu de cela, nous allons définir les classes dîner et RSVP, comment ils cartographient à / à partir de la base de données, et les relations entre eux. LINQ à SQL s’occupera alors de générer la logique d’exécution SQL appropriée à utiliser au moment de l’exécution lorsque nous interagissons et les utilisons.

Nous pouvons utiliser le support linguistique LINQ au sein de VB et C pour écrire des requêtes expressives qui récupèrent les objets Dîner et RSVP de la base de données. Cela minimise la quantité de code de données que nous devons écrire, et nous permet de construire des applications vraiment propres.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Ajout de LINQ à SQL Classes à notre projet

Nous commencerons par cliquer à droite sur le dossier "Modèles" dans notre projet, et sélectionner la commande de menu **Add-&gt;New Item:**

![](build-a-model-with-business-rule-validations/_static/image1.png)

Cela fera ressortir le dialogue "Ajouter un nouvel article". Nous filtrerons par la catégorie « Données » et sélectionnerons le modèle « LINQ to SQL Classes » en son sein :

![](build-a-model-with-business-rule-validations/_static/image2.png)

Nous nommerons l’élément "NerdDinner" et cliquez sur le bouton "Ajouter". Visual Studio ajoutera un fichier NerdDinner.dbml sous notre répertoire de modèles, puis ouvrira le LINQ au concepteur relationnel d’objet DE SQL :

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Création de classes de modèles de données avec LINQ à SQL

LINQ à SQL nous permet de créer rapidement des classes de modèles de données à partir du schéma de base de données existant. Pour ce faire, nous allons ouvrir la base de données NerdDinner dans le Server Explorer, et sélectionner les tables que nous voulons modéliser en elle:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Nous pouvons ensuite faire glisser les tables sur la surface de concepteur LINQ à SQL designer. Lorsque nous faisons ce LINQ à SQL sera automatiquement créer des classes de dîner et RSVP en utilisant le schéma des tables (avec des propriétés de classe qui cartographient les colonnes de table de base de données):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Par défaut, le LINQ au concepteur SQL « pluralise » automatiquement les noms de table et de colonnes lorsqu’il crée des classes en fonction d’un schéma de base de données. Par exemple : la table « Dîners » dans notre exemple ci-dessus a donné lieu à un cours de « dîner ». Ce nommage de classe aide à rendre nos modèles compatibles avec les conventions de nommage .NET, et je trouve habituellement que d’avoir le concepteur fixer ce pratique (surtout lors de l’ajout de beaucoup de tables). Si vous n’aimez pas le nom d’une classe ou d’une propriété que le concepteur génère, cependant, vous pouvez toujours le remplacer et le changer à n’importe quel nom que vous voulez. Vous pouvez le faire soit en éditant l’entité / nom de propriété en ligne au sein du concepteur ou en le modifiant via la grille de propriété.

Par défaut, le concepteur de LINQ à SQL inspecte également les principales relations clés clés clés/étrangères des tableaux, et en fonction d’elles crée automatiquement des « associations relationnelles » par défaut entre les différentes classes modèles qu’il crée. Par exemple, lorsque nous avons traîné les tables dîners et RSVP sur le LINQ au concepteur SQL une association de relations à un à plusieurs entre les deux a été déduite en fonction du fait que la table RSVP avait une clé étrangère à la table des dîners (ce qui est indiqué par la flèche dans le concepteur):

![](build-a-model-with-business-rule-validations/_static/image6.png)

L’association ci-dessus amènera LINQ à SQL à ajouter une propriété « Dîner » fortement tapée à la classe RSVP que les promoteurs peuvent utiliser pour accéder au dîner associé à un RSVP donné. Il fera également la classe Dîner d’avoir un "RSVP" propriété de collecte qui permet aux développeurs de récupérer et de mettre à jour les objets RSVP associés à un dîner particulier.

Ci-dessous vous pouvez voir un exemple d’intellisense au sein de Visual Studio lorsque nous créons un nouvel objet RSVP et l’ajouter à une collection de RSVP d’un dîner. Remarquez comment LINQ à SQL a automatiquement ajouté une collection « RSVPs » sur l’objet Dîner :

![](build-a-model-with-business-rule-validations/_static/image7.png)

En ajoutant l’objet RSVP à la collection RSVP du dîner, nous demandons à LINQ à SQL d’associer une relation à clé à l’étranger entre le dîner et la ligne RSVP dans notre base de données :

![](build-a-model-with-business-rule-validations/_static/image8.png)

Si vous n’aimez pas la façon dont le concepteur a modélisé ou nommé une association de table, vous pouvez l’emporter. Il suffit de cliquer sur la flèche de l’association au sein du concepteur et d’accéder à ses propriétés via le réseau de propriété pour renommer, supprimer ou modifier. Pour notre application NerdDinner, cependant, les règles d’association par défaut fonctionnent bien pour les classes de modèle de données que nous construisons et nous pouvons simplement utiliser le comportement par défaut.

### <a name="nerddinnerdatacontext-class"></a>Classe NerdDinnerDataContext

Visual Studio créera automatiquement des classes .NET qui représentent les modèles et les relations de base de données définis à l’aide du LINQ au concepteur SQL. Une classe LINQ à SQL DataContext est également générée pour chaque fichier LINQ à SQL designer ajouté à la solution. Parce que nous avons nommé notre LINQ à l’article de classe SQL "NerdDinner", la classe DataContext créée s’appellera "NerdDinnerDataContext". Cette classe NerdDinnerDataContext est la principale façon d’interagir avec la base de données.

Notre classe NerdDinnerDataContext expose deux propriétés - "Dîners" et "RSVPs" - qui représentent les deux tables que nous avons modélisées dans la base de données. Nous pouvons utiliser C pour écrire des requêtes LINQ contre ces propriétés pour interroger et récupérer les objets Dîner et RSVP de la base de données.

Le code suivant montre comment instantané un objet NerdDinnerDataContext et effectuer une requête LINQ contre lui pour obtenir une séquence de dîners qui se produisent à l’avenir. Visual Studio fournit intellisense complet lors de l’écriture de la requête LINQ, et les objets qui en reviennent sont fortement dactylographiés et soutiennent également l’intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

En plus de nous permettre d’interroger pour les objets Dîner et RSVP, un nerdDinnerDataContext suit également automatiquement toutes les modifications que nous faisons par la suite pour le dîner et les objets RSVP que nous récupérons à travers elle. Nous pouvons utiliser cette fonctionnalité pour enregistrer facilement les modifications vers la base de données - sans avoir à écrire un code de mise à jour SQL explicite.

Par exemple, le code ci-dessous montre comment utiliser une requête LINQ pour récupérer un seul objet dîner de la base de données, mettre à jour deux des propriétés du dîner, puis enregistrer les modifications à la base de données :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

L’objet NerdDinnerDataContext dans le code ci-dessus a automatiquement suivi les modifications de propriété apportées à l’objet Dinner que nous avons récupéré de celui-ci. Lorsque nous avons appelé la méthode « SubmitChanges()», elle exécutera une déclaration appropriée de SQL « UPDATE » à la base de données pour maintenir les valeurs mises à jour.

### <a name="creating-a-dinnerrepository-class"></a>Création d’une classe DinnerRepository

Pour les petites applications, il est parfois très bien que les contrôleurs travaillent directement contre un LINQ à la classe DataContext SQL, et intègrent des requêtes LINQ au sein des contrôleurs. Cependant, à mesure que les applications s’agrandient, cette approche devient lourde à maintenir et à tester. Il peut aussi nous conduire à dupliquer les mêmes requêtes LINQ à plusieurs endroits.

Une approche qui peut rendre les applications plus faciles à maintenir et à tester est d’utiliser un modèle de « référentiel ». Une classe de référentiel aide à encapsuler la logique de requête et de persistance des données, et résume les détails de mise en œuvre de la persistance des données de l’application. En plus de rendre le code d’application plus propre, l’utilisation d’un modèle de référentiel peut faciliter la modification des implémentations de stockage de données à l’avenir, et il peut aider à faciliter le test unitaire d’une application sans avoir besoin d’une véritable base de données.

Pour notre application NerdDinner, nous définirons un cours DinnerRepository avec la signature ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Remarque : Plus tard dans ce chapitre, nous extrairons une interface IDinnerRepository de cette classe et permettons l’injection de dépendance avec elle sur nos contrôleurs. Pour commencer, cependant, nous allons commencer simple et juste travailler directement avec la classe DinnerRepository.*

Pour implémenter cette classe, nous allons cliquer à droite sur notre dossier "Modèles" et choisir la commande de menu **Add-&gt;New Item.** Dans le dialogue " Ajouter un nouvel article ", nous sélectionnerons le modèle de la "Classe" et nommerons le fichier "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Nous pouvons ensuite implémenter notre classe DinnerRepository en utilisant le code ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Récupération, mise à jour, insertion et suppression à l’aide de la classe DinnerRepository

Maintenant que nous avons créé notre classe DinnerRepository, regardons quelques exemples de code qui démontrent des tâches communes que nous pouvons faire avec elle:

#### <a name="querying-examples"></a>Exemples de requête

Le code ci-dessous récupère un seul dîner en utilisant la valeur DinnerID :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Le code ci-dessous récupère tous les dîners et boucles à venir sur eux:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Insérer et mettre à jour des exemples

Le code ci-dessous démontre l’ajout de deux nouveaux dîners. Les ajouts/modifications au référentiel ne sont pas validés dans la base de données tant que la méthode « Enregistrer)» n’est pas demandée. LINQ à SQL enveloppe automatiquement tous les changements dans une transaction de base de données , de sorte que soit toutes les modifications se produisent ou aucun d’entre eux ne le font lorsque notre référentiel enregistre :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Le code ci-dessous récupère un objet dîner existant et modifie deux propriétés sur celui-ci. Les modifications sont réenregistres à la base de données lorsque la méthode « Enregistrer)» est appelée sur notre référentiel :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Le code ci-dessous récupère un dîner, puis y ajoute un RSVP. Il le fait en utilisant la collection RSVPs sur l’objet Dîner que LINQ à SQL créé pour nous (parce qu’il ya une clé primaire / relation à clé étrangère entre les deux dans la base de données). Ce changement est persisté à la base de données comme une nouvelle ligne de table RSVP lorsque la méthode "Enregistrer)" est appelée sur le référentiel:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Supprimer l’exemple

Le code ci-dessous récupère un objet dîner existant, puis le marque pour être supprimé. Lorsque la méthode « Enregistrer)» est appelée sur le référentiel, elle s’engage à la supprimer vers la base de données :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Intégration de la logique de validation et de règle d’entreprise avec les catégories de modèles

L’intégration de la logique de validation et de règle d’entreprise est un élément clé de toute application qui fonctionne avec les données.

#### <a name="schema-validation"></a>Validation de schéma

Lorsque les classes de modèles sont définies à l’aide du concepteur LINQ à SQL, les datatypes des propriétés des classes de modèles de données correspondent aux datatypes de la table de base de données. Par exemple : si la colonne « EventDate » dans la table des dîners est une « datetime », la classe de modèle de données créée par LINQ à SQL sera de type « DateTime » (qui est un datatype .NET intégré). Cela signifie que vous obtiendrez des erreurs de compilation si vous essayez d’attribuer un integer ou boolean à elle à partir du code, et il soulèvera une erreur automatiquement si vous essayez de convertir implicitement un type de chaîne non valide à elle au moment de l’exécution.

LINQ à SQL gère également automatiquement les valeurs SQL qui vous échappent lors de l’utilisation des cordes, ce qui vous aidera à vous protéger contre les attaques d’injection SQL lors de son utilisation.

#### <a name="validation-and-business-rule-logic"></a>Validation et logique de règle d’affaires

La validation de Schema est utile comme première étape, mais est rarement suffisante. La plupart des scénarios réels exigent la possibilité de spécifier une logique de validation plus riche qui peut s’étendre sur plusieurs propriétés, exécuter du code et souvent avoir conscience de l’état d’un modèle (par exemple: est-il créé /mis à jour / supprimé, ou dans un état spécifique au domaine comme "archivé"). Il existe une variété de modèles et de cadres différents qui peuvent être utilisés pour définir et appliquer des règles de validation aux classes modèles, et il existe plusieurs cadres basés .NET là-bas qui peuvent être utilisés pour aider à cela. Vous pouvez utiliser à peu près n’importe lequel d’entre eux dans ASP.NET applications MVC.

Aux fins de notre application NerdDinner, nous utiliserons un modèle relativement simple et direct où nous exposons une propriété IsValid et une méthode GetRuleViolations() sur notre objet modèle Dinner. La propriété IsValid retournera vraie ou fausse selon que les règles de validation et d’affaires sont toutes valides. La méthode GetRuleViolations() retournera une liste de toutes les erreurs de règles.

Nous metndons en œuvre IsValid et GetRuleViolations() pour notre modèle De dîner en ajoutant une « classe partielle » à notre projet. Des cours partiels peuvent être utilisés pour ajouter des méthodes/propriétés/événements aux classes maintenues par un concepteur VS (comme la classe dîner générée par le LINQ au concepteur SQL) et aider à éviter l’outil de jouer avec notre code. Nous pouvons ajouter une nouvelle classe partielle à notre projet en cliquant à droite sur le dossier «Modèles», puis sélectionner la commande de menu « Ajouter un nouvel élément ». Nous pouvons alors choisir le modèle "Classe" dans le dialogue "Ajouter un nouvel article" et le nommer Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

En cliquant sur le bouton "Ajouter" ajoutera un fichier Dinner.cs à notre projet et l’ouvrira dans l’IDE. Nous pouvons ensuite mettre en œuvre un cadre de base d’application des règles/validations à l’aide du code ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Quelques notes sur le code ci-dessus:

- La classe dîner est préfacée avec un mot-clé « partiel », ce qui signifie que le code contenu à l’intérieur sera combiné avec la classe générée/maintenue par le LINQ au concepteur SQL et compilée en une seule classe.
- La classe RuleViolation est une classe d’aide que nous allons ajouter au projet qui nous permet de fournir plus de détails sur une violation de règle.
- La méthode Dinner.GetRuleViolations () fait évaluer nos règles de validation et nos règles commerciales (nous les metrons en œuvre sous peu). Il renvoie ensuite une séquence d’objets RuleViolation qui fournissent plus de détails sur les erreurs de règles.
- La propriété Dinner.IsValid offre une propriété d’aide pratique qui indique si l’objet dinner a des règles actives. Il peut être vérifié de manière proactive par un développeur à l’aide de l’objet Dinner à tout moment (et ne soulève pas une exception).
- La méthode partielle Dinner.OnValidate() est un crochet que LINQ à SQL fournit qui nous permet d’être avisés chaque fois que l’objet Dîner est sur le point d’être persisté dans la base de données. Notre mise en œuvre OnValidate () ci-dessus garantit que le dîner n’a pas de règles avant qu’il ne soit enregistré. S’il est dans un état invalide, il soulève une exception, ce qui amènera LINQ à SQL à annuler la transaction.

Cette approche fournit un cadre simple dans lequel nous pouvons intégrer la validation et les règles commerciales. Pour l’instant, ajoutons les règles ci-dessous à notre méthode GetRuleViolations () :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Nous utilisons la fonction « rendement » de C pour retourner une séquence de toutes les règles. Les six premiers contrôles de règle ci-dessus appliquent simplement que les propriétés de chaîne sur notre dîner ne peuvent pas être nulles ou vides. La dernière règle est un peu plus intéressante, et appelle une méthode d’aide PhoneValidator.IsValidNumber() que nous pouvons ajouter à notre projet pour vérifier que le format de numéro ContactPhone correspond au pays du dîner.

Nous pouvons utiliser . Le support d’expression régulier de NET pour implémenter ce support de validation téléphonique. Voici une simple implémentation de PhoneValidator que nous pouvons ajouter à notre projet qui nous permet d’ajouter des contrôles de modèle Regex spécifiques au pays :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Gestion de la validation et des violations de la logique d’entreprise

Maintenant que nous avons ajouté le code de validation et de règle d’entreprise ci-dessus, chaque fois que nous essayons de créer ou de mettre à jour un dîner, nos règles logiques de validation seront évaluées et appliquées.

Les développeurs peuvent écrire du code comme ci-dessous pour déterminer de manière proactive si un objet De dîner est valide, et récupérer une liste de toutes les violations en elle sans soulever d’exceptions:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Si nous essayons de sauver un dîner dans un état invalide, une exception sera soulevée lorsque nous appelons la méthode Save() sur le DinnerRepository. Cela se produit parce que LINQ à SQL appelle automatiquement notre méthode partielle Dinner.OnValidate() avant qu’il enregistre les changements du dîner, et nous avons ajouté du code à Dinner.OnValidate() pour soulever une exception si des violations de règles existent dans le dîner. Nous pouvons attraper cette exception et récupérer de manière réactive une liste des violations à corriger:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Étant donné que nos règles de validation et d’entreprise sont mises en œuvre dans notre couche de modèle, et non dans la couche d’interface utilisateur, elles seront appliquées et utilisées dans tous les scénarios de notre application. Nous pouvons plus tard modifier ou ajouter des règles d’affaires et avoir tout le code qui fonctionne avec nos objets Dîner les honorer.

Avoir la flexibilité de changer les règles d’affaires en un seul endroit, sans que ces changements ne se répercutent tout au long de la logique de l’application et de l’assurance-chômage, est le signe d’une application bien écrite et d’un avantage qu’un cadre MVC aide à encourager.

### <a name="next-step"></a>étape suivante

Nous avons maintenant un modèle que nous pouvons utiliser à la fois pour interroger et mettre à jour notre base de données.

Ajoutons maintenant quelques contrôleurs et vues au projet que nous pouvons utiliser pour construire une expérience d’interface utilisateur HTML autour d’elle.

> [!div class="step-by-step"]
> [Suivant précédent](create-a-database.md)
> [Next](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
