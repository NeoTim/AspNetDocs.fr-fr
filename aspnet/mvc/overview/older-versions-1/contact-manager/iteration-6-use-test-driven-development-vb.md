---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Itération #6 - Utiliser le développement axé sur les tests (VB) Microsoft Docs'
author: rick-anderson
description: Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code contre les tests unitaires. Dans cette itération,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 8464f4cdee673ef75d561e4cf89613fdca2c16af
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542805"
---
# <a name="iteration-6--use-test-driven-development-vb"></a>Itération #6 : Utiliser le développement piloté par les tests (VB)

par [Microsoft](https://github.com/microsoft)

[Code de téléchargement](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code contre les tests unitaires. Dans cette itération, nous ajoutons des groupes de contact.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Construire une application de gestion des contacts ASP.NET MVC (VB)

Dans cette série de tutoriels, nous construisons toute une application de gestion de contact du début à la fin. L’application Contact Manager vous permet de stocker les coordonnées - noms, numéros de téléphone et adresses e-mail - pour une liste de personnes.

Nous construisons l’application sur plusieurs itérations. À chaque itération, nous améliorons progressivement l’application. Le but de cette approche d’itération multiple est de vous permettre de comprendre la raison de chaque changement.

- Itération #1 - Créer l’application. Dans la première itération, nous créons le Gestionnaire de Contact de la manière la plus simple possible. Nous ajoutons un support pour les opérations de base de base de base : Créer, lire, mettre à jour et supprimer (CRUD).

- Itération #2 - Rendre l’application belle. Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page principale de vue par défaut ASP.NET MVC et la feuille de style en cascade.

- Itération #3 - Ajouter la validation du formulaire. Dans la troisième itération, nous ajoutons la validation de forme de base. Nous empêchons les gens de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses e-mail et les numéros de téléphone.

- Itération #4 - Rendre l’application lâchement couplée. Dans cette quatrième itération, nous profitons de plusieurs modèles de conception logicielle pour faciliter le maintien et la modification de l’application Contact Manager. Par exemple, nous refactorrons notre application pour utiliser le modèle de dépôt et le modèle d’injection de dépendance.

- Itération #5 - Créer des tests unitaires. Dans la cinquième itération, nous rendons notre application plus facile à maintenir et à modifier en ajoutant des tests unitaires. Nous nous moquons de nos classes de modèles de données et construisons des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6 - Utilisez le développement axé sur les tests. Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code contre les tests unitaires. Dans cette itération, nous ajoutons des groupes de contact.

- Itération #7 - Ajouter la fonctionnalité Ajax. Dans la septième itération, nous améliorons la réactivité et la performance de notre application en ajoutant un soutien pour Ajax.

## <a name="this-iteration"></a>Cette Itération

Lors de l’itération précédente de l’application Contact Manager, nous avons créé des tests unitaires pour fournir un filet de sécurité pour notre code. La motivation pour créer les tests unitaires était de rendre notre code plus résistant au changement. Avec des tests unitaires en place, nous pouvons heureusement apporter n’importe quelle modification à notre code et savoir immédiatement si nous avons cassé la fonctionnalité existante.

Dans cette itération, nous utilisons des tests unitaires à des fins entièrement différentes. Dans cette itération, nous utilisons des tests unitaires dans le cadre d’une philosophie de conception d’applications appelée *développement piloté par les tests.* Lorsque vous pratiquez le développement axé sur les tests, vous écrivez d’abord des tests, puis écrivez du code contre les tests.

Plus précisément, lorsque vous pratiquez le développement piloté par les tests, il y a trois étapes que vous remplissez lors de la création de code (Rouge/Vert/Refactor) :

1. Écrivez un test unitaire qui échoue (Rouge)
2. Écrire du code qui passe le test unitaire (vert)
3. Refactor votre code (Refactor)

Tout d’abord, vous écrivez le test unitaire. Le test unitaire doit exprimer votre intention quant à la façon dont vous vous attendez à ce que votre code se comporte. Lorsque vous créez le test unitaire pour la première fois, le test unitaire doit échouer. Le test doit échouer parce que vous n’avez pas encore écrit de code d’application qui satisfait le test.

Ensuite, vous écrivez juste assez de code pour que le test unitaire passe. Le but est d’écrire le code de la manière la plus paresseuse, la plus bâclée et la plus rapide possible. Vous ne devriez pas perdre de temps à penser à l’architecture de votre application. Au lieu de cela, vous devriez vous concentrer sur la rédaction de la quantité minimale de code nécessaire pour satisfaire l’intention exprimée par le test unitaire.

Enfin, après avoir écrit suffisamment de code, vous pouvez prendre du recul et considérer l’architecture globale de votre application. Dans cette étape, vous réécrivez (refactor) votre code en profitant des modèles de conception logicielle - tels que le modèle de dépôt - de sorte que votre code est plus maintenable. Vous pouvez réécrire sans crainte votre code dans cette étape parce que votre code est couvert par des tests unitaires.

Il y a beaucoup d’avantages qui résultent de la pratique du développement axé sur les tests. Tout d’abord, le développement axé sur les tests vous oblige à vous concentrer sur le code qui doit réellement être écrit. Parce que vous êtes constamment concentré sur l’écriture juste assez de code pour passer un test particulier, vous êtes empêché d’errer dans les herbes et d’écrire des quantités massives de code que vous n’utiliserez jamais.

Deuxièmement, une méthodologie de conception « test d’abord » vous oblige à écrire du code du point de vue de la façon dont votre code sera utilisé. En d’autres termes, lorsque vous pratiquez le développement axé sur les tests, vous écrivez constamment vos tests du point de vue de l’utilisateur. Par conséquent, le développement axé sur les tests peut entraîner des API plus propres et plus compréhensibles.

Enfin, le développement piloté par les tests vous oblige à écrire des tests unitaires dans le cadre du processus normal d’écriture d’une demande. À l’approche de la date limite du projet, le test est généralement la première chose qui sort par la fenêtre. Lors de la pratique du développement axé sur les tests, d’autre part, vous êtes plus susceptible d’être vertueux sur la rédaction de tests unitaires parce que le développement piloté par test rend les tests unitaires au cœur du processus de construction d’une application.

> [!NOTE] 
> 
> Pour en savoir plus sur le développement axé sur les tests, je vous recommande de lire Michael Feathers livre **Working Effectively with Legacy Code**.

Dans cette itération, nous ajoutons une nouvelle fonctionnalité à notre application Contact Manager. Nous ajoutons un soutien aux groupes de contact. Vous pouvez utiliser les groupes de contact pour organiser vos contacts en catégories telles que les groupes Business et Friend.

Nous ajouterons cette nouvelle fonctionnalité à notre application en suivant un processus de développement piloté par les tests. Nous écrirons d’abord nos tests unitaires et nous écrirons tout notre code contre ces tests.

## <a name="what-gets-tested"></a>Ce qui est testé

Comme nous l’avons mentionné dans l’itération précédente, vous n’écrivez généralement pas de tests unitaires pour la logique d’accès aux données ou de voir la logique. Vous n’écrivez pas de tests unitaires pour la logique d’accès aux données parce que l’accès à une base de données est une opération relativement lente. Vous n’écrivez pas de tests unitaires pour la logique de vue parce que l’accès à une vue nécessite de faire tourner vers le haut d’un serveur web qui est une opération relativement lente. Vous ne devriez pas écrire un test unitaire à moins que le test puisse être exécuté encore et encore très rapidement

Parce que le développement axé sur les tests est conduit par des tests unitaires, nous nous concentrons d’abord sur la rédaction de contrôleur et la logique d’affaires. Nous évitons de toucher la base de données ou les vues. Nous ne modifierons pas la base de données ou ne créerons pas nos vues avant la toute fin de ce tutoriel. Nous commençons par ce qui peut être testé.

## <a name="creating-user-stories"></a>Création d’histoires d’utilisateurs

Lorsque vous pratiquez le développement axé sur les tests, vous commencez toujours par écrire un test. Cela soulève immédiatement la question: Comment décidez-vous quel test écrire en premier? Pour répondre à cette question, vous devriez écrire un ensemble [*d’histoires d’utilisateurs*](http://en.wikipedia.org/wiki/User_stories).

Une histoire d’utilisateur est une description très brève (généralement une phrase) d’une exigence logicielle. Il devrait s’agir d’une description non technique d’une exigence écrite du point de vue de l’utilisateur.

Voici l’ensemble d’histoires d’utilisateurs qui décrivent les fonctionnalités requises par la nouvelle fonctionnalité du Groupe de contact :

1. L’utilisateur peut consulter une liste de groupes de contacts.
2. L’utilisateur peut créer un nouveau groupe de contact.
3. L’utilisateur peut supprimer un groupe de contact existant.
4. L’utilisateur peut sélectionner un groupe de contact lors de la création d’un nouveau contact.
5. L’utilisateur peut sélectionner un groupe de contact lors de l’édition d’un contact existant.
6. Une liste des groupes de contact est affichée dans la vue de l’index.
7. Lorsqu’un utilisateur clique sur un groupe de contact, une liste de contacts correspondants s’affiche.

Notez que cette liste d’histoires d’utilisateurs est tout à fait compréhensible par un client. Il n’y a aucune mention des détails de la mise en œuvre technique.

Pendant la construction de votre application, l’ensemble des histoires d’utilisateurs peut devenir plus raffiné. Vous pouvez décomposer une histoire d’utilisateur en plusieurs histoires (exigences). Par exemple, vous pouvez décider que la création d’un nouveau groupe de contact devrait impliquer la validation. Soumettre un groupe de contact sans nom doit renvoyer une erreur de validation.

Après avoir créé une liste d’histoires d’utilisateurs, vous êtes prêt à écrire votre premier test unitaire. Nous commencerons par créer un test unitaire pour consulter la liste des groupes de contact.

## <a name="listing-contact-groups"></a>Liste des groupes de contact

Notre première histoire d’utilisateur est qu’un utilisateur doit être en mesure de voir une liste de groupes de contacts. Nous devons exprimer cette histoire avec un test.

Créez un nouveau test unitaire en cliquant à droite sur le dossier Controllers dans le projet ContactManager.Tests, en sélectionnant **Add, New Test**et en sélectionnant le modèle **de test unitaire** (voir la figure 1). Nommez le nouveau test unitaire GroupControllerTest.vb et cliquez sur le bouton **OK.**

[![Ajout du test unitaire GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Figure 01**: Ajout du test unitaire GroupControllerTest[(Cliquez pour voir l’image grandeur nature)](iteration-6-use-test-driven-development-vb/_static/image2.png)

Notre premier test unitaire est contenu dans la liste 1. Ce test vérifie que la méthode Index() du contrôleur de groupe renvoie un ensemble de groupes. Le test vérifie qu’une collection de groupes est retournée en vue des données.

**Liste 1 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Lorsque vous tapez le code dans la liste 1 dans Visual Studio, vous obtiendrez beaucoup de lignes rouges squiggly. Nous n’avons pas créé les classes GroupController ou Group.

À ce stade, nous ne pouvons même pas construire notre application afin que nous puissions ne pas exécuter notre premier test unitaire. C’est bon. Cela compte comme un test défaillant. Par conséquent, nous avons maintenant la permission de commencer à écrire le code d’application. Nous devons écrire suffisamment de code pour exécuter notre test.

La classe de contrôleur de groupe dans la liste 2 contient le strict minimum de code requis pour réussir le test unitaire. L’action Index() renvoie une liste statique de groupes codés (la classe du Groupe est définie dans la liste 3).

**Liste 2 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Liste 3 - Models-Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Après avoir ajouté les classes GroupController et Group à notre projet, notre premier test unitaire se termine avec succès (voir la figure 2). Nous avons fait le travail minimum nécessaire pour passer le test. Il est temps de célébrer.

[![Succès!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Figure 02**: Succès! ([Cliquez pour voir l’image pleine grandeur](iteration-6-use-test-driven-development-vb/_static/image4.png))

## <a name="creating-contact-groups"></a>Création de groupes de contact

Maintenant, nous pouvons passer à la deuxième histoire d’utilisateur. Nous devons être en mesure de créer de nouveaux groupes de contact. Nous devons exprimer cette intention par un test.

Le test de La Liste 4 vérifie que l’appel de la méthode Créer () avec un nouveau Groupe ajoute le Groupe à la liste des groupes retournés par la méthode Index(). En d’autres termes, si je crée un nouveau groupe, je devrais être en mesure de récupérer le nouveau groupe de la liste des groupes retournés par la méthode Index() .

**Liste 4 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Le test dans La liste 4 appelle la méthode De création de contrôleur de groupe avec un nouveau groupe de contact. Ensuite, le test vérifie que l’appel de la méthode Group Controller Index() renvoie le nouveau Groupe en vue des données.

Le contrôleur de groupe modifié dans la liste 5 contient le strict minimum de modifications requises pour réussir le nouveau test.

**Liste 5 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Le contrôleur de groupe dans La liste 5 a une nouvelle action Create(). Cette action ajoute un Groupe à une collection de groupes. Notez que l’action Index() a été modifiée pour retourner le contenu de la collection des groupes.

Encore une fois, nous avons effectué le minimum de travail nécessaire pour réussir le test unitaire. Après avoir apporté ces modifications au contrôleur de groupe, tous nos tests unitaires passent.

## <a name="adding-validation"></a>Ajouter une validation

Cette exigence n’a pas été énoncée explicitement dans l’histoire de l’utilisateur. Toutefois, il est raisonnable d’exiger qu’un Groupe ait un nom. Sinon, l’organisation de contacts en groupes ne serait pas très utile.

Liste 6 contient un nouveau test qui exprime cette intention. Ce test vérifie que tenter de créer un groupe sans fournir de nom aboutit à un message d’erreur de validation dans l’état du modèle.

**Liste 6 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Afin de satisfaire ce test, nous devons ajouter une propriété Nom à notre classe de groupe (voir Liste 7). En outre, nous devons ajouter un tout petit peu de logique de validation à notre contrôleur de groupe s Créer () action (voir Liste 8).

**Liste 7 - Models-Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Liste 8 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Notez que l’action De création () du contrôleur de groupe contient désormais à la fois la validation et la logique de base de données. Actuellement, la base de données utilisée par le contrôleur de groupe se compose de rien de plus qu’une collection en mémoire.

## <a name="time-to-refactor"></a>Temps de Refactor

La troisième étape dans Red/Green/Refactor est la partie Refactor. À ce stade, nous devons prendre du recul par chemin de notre code et réfléchir à la façon dont nous pouvons refactorer notre application pour améliorer sa conception. L’étape Refactor est l’étape à laquelle nous pensons dur sur la meilleure façon de mettre en œuvre des principes et des modèles de conception de logiciels.

Nous sommes libres de modifier notre code de la manière que nous choisissons d’améliorer la conception du code. Nous disposons d’un filet de sécurité des tests unitaires qui nous empêchent de briser les fonctionnalités existantes.

À l’heure actuelle, notre contrôleur de groupe est un gâchis du point de vue d’une bonne conception de logiciels. Le contrôleur du groupe contient un désordre enchevêtré de validation et de code d’accès aux données. Pour éviter de violer le principe de responsabilité unique, nous devons séparer ces préoccupations en différentes classes.

Notre classe de contrôleur de groupe refactorisé est contenue dans la liste 9. Le contrôleur a été modifié pour utiliser la couche de service ContactManager. Il s’agit de la même couche de service que nous utilisons avec le contrôleur de contact.

La liste 10 contient les nouvelles méthodes ajoutées à la couche de service ContactManager pour soutenir la validation, la liste et la création de groupes. L’interface IContactManagerService a été mise à jour pour inclure les nouvelles méthodes.

La liste 11 contient une nouvelle classe FakeContactManagerRepository qui met en œuvre l’interface IContactManagerRepository. Contrairement à la classe EntityContactManagerRepository qui met également en œuvre l’interface IContactManagerRepository, notre nouvelle classe FakeContactManagerRepository ne communique pas avec la base de données. La classe FakeContactManagerRepository utilise une collection de mémoire comme proxy pour la base de données. Nous utiliserons cette classe dans nos tests unitaires comme une fausse couche de dépôt.

**Liste 9 - Controllers-GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Liste 10 - Controllers-ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Liste 11 - Controllers-FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]

La modification de l’interface IContactManagerRepository nécessite une utilisation pour implémenter les méthodes CreateGroup() et ListGroups() dans la classe EntityContactManagerRepository. La façon la plus paresseuse et la plus rapide de le faire est d’ajouter des méthodes de talon qui ressemblent à ceci:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]

Enfin, ces changements à la conception de notre application nous obligent à apporter quelques modifications à nos tests unitaires. Nous devons maintenant utiliser le FakeContactManagerRepository lors de l’exécution des tests unitaires. La classe GroupControllerTest mise à jour est contenue dans la liste 12.

**Liste 12 - Controllers-GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Après avoir fait tous ces changements, une fois de plus, tous nos tests unitaires passent. Nous avons terminé tout le cycle de Red/Green/Refactor. Nous avons mis en œuvre les deux premières histoires d’utilisateurs. Nous avons maintenant des tests unitaires à l’appui pour les exigences exprimées dans les histoires d’utilisateurs. La mise en œuvre du reste des histoires d’utilisateurs implique de répéter le même cycle de Red/Green/Refactor.

## <a name="modifying-our-database"></a>Modification de notre base de données

Malheureusement, même si nous avons satisfait à toutes les exigences exprimées par nos tests unitaires, notre travail n’est pas terminé. Nous devons encore modifier notre base de données.

Nous devons créer une nouvelle table de base de données du Groupe. Procédez comme suit :

1. Dans la fenêtre Server Explorer, cliquez à droite sur le dossier Tables et sélectionnez l’option menu **Ajouter une nouvelle table**.
2. Entrez les deux colonnes décrites ci-dessous dans le concepteur de table.
3. Marquez la colonne Id comme clé principale et colonne d’identité.
4. Enregistrer la nouvelle table avec le nom Groupes en cliquant sur l’icône de la disquette.

<a id="0.12_table01"></a>

| **Nom de colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | int | False |
| Nom | nvarchar(50) | False |

Ensuite, nous devons supprimer toutes les données de la table Contacts (sinon, nous ne serons pas en mesure de créer une relation entre les tableaux Contacts et Groupes). Procédez comme suit :

1. Cliquez à droite sur la table Contacts et sélectionnez l’option menu **Afficher les données de table**.
2. Supprimer toutes les lignes.

Ensuite, nous devons définir une relation entre le tableau de base de données des groupes et le tableau de base de données Contacts existant. Procédez comme suit :

1. Double-cliquez sur la table Contacts dans la fenêtre Server Explorer pour ouvrir le concepteur de table.
2. Ajoutez une nouvelle colonne d’intégrer à la table Contacts nommée GroupId.
3. Cliquez sur le bouton Relation pour ouvrir le dialogue sur les relations avec les relations avec les clés étrangères (voir la figure 3).
4. Cliquez sur le bouton Ajouter.
5. Cliquez sur le bouton ellipsis qui apparaît à côté du bouton De spécification de la table et des colonnes.
6. Dans le dialogue Tableaux et Colonnes, sélectionnez les groupes comme tableau principal et Id comme colonne principale. Sélectionnez Contacts comme tableau clé étranger et GroupId comme colonne clé étrangère (voir la figure 4). Cliquez sur le bouton OK.
7. Dans le cadre **de la spécification INSERT et UPDATE**, sélectionnez la valeur **Cascade** pour supprimer **la règle**.
8. Cliquez sur le bouton Fermer pour fermer le dialogue Foreign Key Relationships.
9. Cliquez sur le bouton Enregistrer pour enregistrer les modifications apportées à la table Contacts.

[![Création d’une relation de table de base de données](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Figure 03**: Création d’une relation de table de base de données[(Cliquez pour voir l’image grandeur nature](iteration-6-use-test-driven-development-vb/_static/image6.png))

[![Spécifier les relations de table](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Figure 04**: Spécifier les relations de table[(Cliquez pour voir l’image grandeur nature](iteration-6-use-test-driven-development-vb/_static/image8.png))

### <a name="updating-our-data-model"></a>Mise à jour de notre modèle de données

Ensuite, nous devons mettre à jour notre modèle de données pour représenter le nouveau tableau de base de données. Procédez comme suit :

1. Double-cliquez sur le fichier ContactManagerModel.edmx dans le dossier Models pour ouvrir le concepteur d’entités.
2. Cliquez à droite sur la surface du Concepteur et sélectionnez le **modèle de mise à jour de l’option de**menu à partir de database .
3. Dans l’assistant de mise à jour, sélectionnez la table groupes et cliquez sur le bouton Finition (voir figure 5).
4. Cliquez à droite sur l’entité Groupes et sélectionnez l’option de menu **Renommer**. Changer le nom de l’entité *Groupes* en *Groupe* (singulier).
5. Cliquez à droite sur la propriété de navigation Des groupes qui apparaît au bas de l’entité Contact. Changer le nom de la propriété de navigation *des Groupes* en *Groupe* (singulier).

[![Mise à jour d’un modèle cadre d’entité à partir de la base de données](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Figure 05**: Mise à jour d’un modèle cadre d’entité à partir de la base de données[(Cliquez pour voir l’image grandeur nature](iteration-6-use-test-driven-development-vb/_static/image10.png))

Une fois que vous aurez terminé ces étapes, votre modèle de données représentera à la fois les tableaux Contacts et Groupes. Le concepteur d’entités doit montrer les deux entités (voir la figure 6).

[![Concepteur d’entité affichant le groupe et le contact](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Figure 06**: Entity Designer affichant le groupe et le contact ([Cliquez pour voir l’image grandeur nature](iteration-6-use-test-driven-development-vb/_static/image12.png))

### <a name="creating-our-repository-classes"></a>Création de nos classes de dépôt

Ensuite, nous devons mettre en œuvre notre classe de dépôt. Au cours de cette itération, nous avons ajouté plusieurs nouvelles méthodes à l’interface IContactManagerRepository tout en écrivant du code pour satisfaire nos tests unitaires. La version finale de l’interface IContactManagerRepository est contenue dans la liste 14.

**Liste 14 - Models-IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Nous n’avons mis en œuvre aucune des méthodes liées au travail avec les groupes de contact dans notre classe de véritable EntityContactManagerRepository. Actuellement, la classe EntityContactManagerRepository a des méthodes de talons pour chacune des méthodes de groupe de contact répertoriées dans l’interface IContactManagerRepository. Par exemple, la méthode ListGroups() ressemble actuellement à ceci :

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Les méthodes de talon nous ont permis de compiler notre application et de passer les tests unitaires. Cependant, il est maintenant temps de mettre en œuvre ces méthodes. La version finale de la classe EntityContactManagerRepository est contenue dans la liste 13.

**Liste 13 - Models-EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Création des vues

ASP.NET’application MVC lorsque vous utilisez le moteur de vue par défaut ASP.NET. Ainsi, vous ne créez pas de vues en réponse à un test unitaire particulier. Cependant, parce qu’une application serait inutile sans vues, nous ne pouvons pas compléter cette itération sans créer et modifier les vues contenues dans l’application Contact Manager.

Nous devons créer les nouveaux points de vue suivants pour la gestion des groupes de contact (voir la figure 7) :

- Views-Group.Index.aspx - Affiche la liste des groupes de contact
- Vues-Groupe-Supprimer.aspx - Affiche le formulaire de confirmation pour la suppression d’un groupe de contact

[![La vue de l’indice du Groupe](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Figure 07**: La vue de l’indice de groupe ([Cliquez pour voir l’image grandeur nature](iteration-6-use-test-driven-development-vb/_static/image14.png))

Nous devons modifier les points de vue existants suivants afin qu’ils comprennent les groupes de contact :

- Vues-Home-Create.aspx
- Vues-Accueil-Edit.aspx
- Vues-Home-Index.aspx

Vous pouvez voir les vues modifiées en regardant l’application Visual Studio qui accompagne ce tutoriel. Par exemple, la figure 8 illustre la vue de l’indice de contact.

[![La vue de l’indice de contact](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Figure 08**: La vue de l’indice de contact ([Cliquez pour voir l’image grandeur nature](iteration-6-use-test-driven-development-vb/_static/image16.png))

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté de nouvelles fonctionnalités à notre application Contact Manager en suivant une méthodologie de conception d’applications de développement axée sur les tests. Nous avons commencé par créer un ensemble d’histoires d’utilisateurs. Nous avons créé un ensemble de tests unitaires qui correspondent aux exigences exprimées par les histoires d’utilisateurs. Enfin, nous avons écrit juste assez de code pour satisfaire aux exigences exprimées par les tests unitaires.

Après avoir terminé l’écriture de suffisamment de code pour satisfaire aux exigences exprimées par les tests unitaires, nous avons mis à jour notre base de données et nos vues. Nous avons ajouté un nouveau tableau des groupes à notre base de données et mis à jour notre modèle de données cadre d’entités. Nous avons également créé et modifié un ensemble de vues.

Dans la prochaine itération - l’itération finale - nous réécrivons notre application pour profiter de l’Ajax. En profitant de l’Ajax, nous améliorerons la réactivité et les performances de l’application Contact Manager.

> [!div class="step-by-step"]
> [Suivant précédent](iteration-5-create-unit-tests-vb.md)
> [Next](iteration-7-add-ajax-functionality-vb.md)
