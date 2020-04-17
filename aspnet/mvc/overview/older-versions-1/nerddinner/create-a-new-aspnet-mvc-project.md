---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Créer un nouveau projet de ASP.NET MVC (fr) Microsoft Docs
author: rick-anderson
description: L’étape 1 vous montre comment mettre en place la structure d’application nerdDinner de base.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 240c8a04cec3c07f434182775d1c519aab029761
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541479"
---
# <a name="create-a-new-aspnet-mvc-project"></a>Créer un projet ASP.NET MVC

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 1 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 1 vous montre comment mettre en place la structure d’application nerdDinner de base.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner Étape 1:&gt;Fichier- Nouveau projet

Nous commencerons notre application NerdDinner en sélectionnant **l’élément&gt;de** menu File- Nouveau Projet dans Visual Studio 2008 ou le Visual Web Developer 2008 Express gratuit.

Cela fera l’histoire du dialogue « Nouveau projet ». Pour créer une nouvelle application ASP.NET MVC, nous sélectionnerons le nœud « Web » sur le côté gauche du dialogue, puis choisirons le modèle de projet « ASP.NET MVC Web Application » à droite :

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Important: Assurez-vous que vous avez téléchargé et installé ASP.NET MVC - sinon il ne sera pas apparaître dans le dialogue Nouveau Projet. Vous pouvez utiliser V2 de l’installateur de [plate-forme Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) si vous ne l’avez pas encore installé (ASP.NET MVC est disponible dans la section « Plateforme Web-&gt;Frameworks et Runtimes »).*

Nous allons nommer le nouveau projet que nous allons créer "NerdDinner" et ensuite cliquer sur le bouton "ok" pour le créer.

Lorsque nous cliquez sur "ok" Visual Studio apportera un dialogue supplémentaire qui nous invite à créer en option un projet de test unitaire pour la nouvelle application ainsi. Ce projet de test unitaire nous permet de créer des tests automatisés qui vérifient la fonctionnalité et le comportement de notre application (quelque chose que nous couvrirons comment faire plus tard dans ce tutoriel).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Le dropdown «cadre d’essai» dans le dialogue ci-dessus est peuplé de tous les modèles disponibles ASP.NET projet de test unitaire MVC installés sur la machine. Les versions peuvent être téléchargées pour NUnit, MBUnit et XUnit. Le cadre intégré visual studio Unit Test est également pris en charge.

*Remarque : Le cadre de test de l’unité de studio visuel n’est disponible qu’avec visual Studio 2008 Versions professionnelles et supérieures. Si vous utilisez VS 2008 Standard Edition ou Visual Web Developer 2008 Express, vous devrez télécharger et installer les extensions NUnit, MBUnit ou XUnit pour ASP.NET MVC afin que ce dialogue soit affiché. Le dialogue ne s’affiche pas s’il n’y a pas de cadres de test installés.*

Nous utiliserons le nom par défaut "NerdDinner.Tests" pour le projet de test que nous créons, et utiliserons l’option cadre "Visual Studio Unit Test". Lorsque nous cliquez sur le bouton "ok" Visual Studio va créer une solution pour nous avec deux projets en elle - un pour notre application web et un pour nos tests unitaires:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examen de la structure du répertoire NerdDinner

Lorsque vous créez une nouvelle application ASP.NET MVC avec Visual Studio, elle ajoute automatiquement un certain nombre de fichiers et d’annuaires au projet :

![](create-a-new-aspnet-mvc-project/_static/image4.png)

ASP.NET projets MVC par défaut ont six répertoires de haut niveau :

| **Directory** | **Objectif** |
| --- | --- |
| **/Contrôleurs** | Où vous mettez des classes Controller qui traitent les demandes d’URL |
| **/Modèles** | Où vous mettez des classes qui représentent et manipulent les données |
| **/Vues** | Lorsque vous mettez des fichiers de modèle d’interface utilisateur qui sont responsables du rendu de la sortie |
| **/Scripts** | Où vous mettez des fichiers et des scripts de bibliothèque JavaScript (.js) |
| **/Contenu** | Où vous mettez des fichiers CSS et images, et d’autres contenus non dynamiques/non-JavaScript |
| **/Données\_d’application** | Où vous stockez des fichiers de données que vous souhaitez lire/écrire. |

ASP.NET MVC n’a pas besoin de cette structure. En fait, les développeurs travaillant sur de grandes applications répartiront généralement l’application à travers plusieurs projets pour la rendre plus gérable (par exemple : les classes de modèles de données vont souvent dans un projet de bibliothèque de classe distinct de l’application Web). La structure de projet par défaut, cependant, fournit une convention d’annuaire par défaut agréable que nous pouvons utiliser pour garder nos préoccupations d’application propre.

Lorsque nous agrandissons le répertoire /Controllers, nous constaterons que Visual Studio a ajouté deux classes de contrôleurs - HomeController et AccountController - par défaut au projet :

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Lorsque nous élargissons l’annuaire /Views, nous trouverons trois sous-répertoires - /Home, /Compte et /Partagé - ainsi que plusieurs fichiers de modèle en leur sein ont également été ajoutés au projet par défaut:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Lorsque nous élargissons les répertoires /Content et /Scripts, nous trouverons un fichier Site.css qui est utilisé pour coiffer tous les HTML sur le site, ainsi que les bibliothèques JavaScript qui peuvent activer ASP.NET’AJAX et le support jQuery dans l’application:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Lorsque nous étendrons le projet NerdDinner.Tests, nous trouverons deux classes qui contiennent des tests unitaires pour nos classes de contrôleur :

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Ces fichiers par défaut ajoutés par Visual Studio nous fournissent une structure de base pour une application de travail - avec page d’accueil, sur la page, la connexion de compte / logout / pages d’enregistrement, et une page d’erreur non manipulée (tous câblés et de travail hors de la boîte).

### <a name="running-the-nerddinner-application"></a>Exécution de l’application NerdDinner

Nous pouvons exécuter le projet en choisissant soit le **&gt;Debug- Start Debugging** ou **Debug-&gt;Start Without Debugging** éléments du menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Cela lancera le serveur Web intégré ASP.NET qui est livré avec Visual Studio, et exécutera notre application :

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Voici la page d’accueil de notre nouveau projet (URL: "/") lorsqu’il s’exécute:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

En cliquant sur l’onglet "A propos" affiche une page sur (URL: "/Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

En cliquant sur le lien "Log On" en haut à droite, nous amène à une page Login (URL: "/Compte/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Si nous n’avons pas de compte de connexion, nous pouvons cliquer sur le lien de registre (URL: "/Compte/Register") pour en créer un :

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Le code pour implémenter la maison ci-dessus, environ, et logout / fonction d’enregistrement a été ajouté par défaut lorsque nous avons créé notre nouveau projet. Nous l’utiliserons comme point de départ de notre application.

### <a name="testing-the-nerddinner-application"></a>Test de l’application NerdDinner

Si nous utilisons l’édition professionnelle ou la version supérieure de Visual Studio 2008, nous pouvons utiliser le support IDE de test unitaire intégré au sein de Visual Studio pour tester le projet :

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Choisir l’une des options ci-dessus ouvrira le volet «Résultats d’essai» au sein de l’IDE et nous donnera un statut de passage/échec sur les 27 tests unitaires inclus dans notre nouveau projet qui couvrent la fonctionnalité intégrée :

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Plus tard dans ce tutoriel, nous allons parler plus de tests automatisés et ajouter des tests unitaires supplémentaires qui couvrent la fonctionnalité d’application que nous mettons en œuvre.

### <a name="next-step"></a>étape suivante

Nous avons maintenant une structure d’application de base en place. Créons maintenant [une base de données pour stocker nos données d’application](create-a-database.md).

> [!div class="step-by-step"]
> [Suivant précédent](introducing-the-nerddinner-tutorial.md)
> [Next](create-a-database.md)
