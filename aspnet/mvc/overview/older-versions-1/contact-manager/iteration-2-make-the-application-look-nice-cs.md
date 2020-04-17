---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: 'Itération #2 - Rendre l’application belle (C) Microsoft Docs'
author: rick-anderson
description: Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page principale de vue par défaut ASP.NET MVC et la feuille de style en cascade.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: ee1d7c92524f6cbdb0f2d7facf85b629e0d91318
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542441"
---
# <a name="iteration-2--make-the-application-look-nice-c"></a>Itération #2 : Donner une belle apparence à l’application (C#)

par [Microsoft](https://github.com/microsoft)

[Code de téléchargement](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page principale de vue par défaut ASP.NET MVC et la feuille de style en cascade.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Construire une application de gestion des contacts ASP.NET MVC (CMD)

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

Le but de cette itération est d’améliorer l’apparence de l’application Contact Manager. À l’heure actuelle, le gestionnaire de contact utilise la page principale de vue par défaut ASP.NET MVC et la feuille de style en cascade (voir la figure 1). Ceux-ci n’ont pas l’air mauvais, mais je ne veux pas que le gestionnaire de contact ressemble à tous les autres ASP.NET site MVC. Je veux remplacer ces fichiers par des fichiers personnalisés.

[![Boîte de dialogue New Project](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figure 01**: L’apparence par défaut d’une application MVC ASP.NET ([Cliquez pour voir l’image grandeur nature](iteration-2-make-the-application-look-nice-cs/_static/image2.png))

Dans cette itération, je discute de deux approches pour améliorer la conception visuelle de notre application. Tout d’abord, je vous montre comment profiter de la galerie ASP.NET MVC Design pour télécharger un modèle de conception ASP.NET gratuit MVC. La galerie ASP.NET MVC Design vous permet de créer une application web professionnelle sans faire de travail.

J’ai décidé de ne pas utiliser un modèle de la galerie ASP.NET MVC Design pour l’application Contact Manager. Au lieu de cela, j’ai eu un design personnalisé créé par une entreprise de design professionnel. Dans la deuxième partie de ce tutoriel, j’explique comment j’ai travaillé avec une société de design professionnel pour créer la ASP.NET conception finale MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>La ASP.NET MVC Design Gallery

La ASP.NET MVC Design Gallery est une ressource gratuite fournie par Microsoft. La galerie ASP.NET MVC est située à l’adresse suivante :

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

La ASP.NET MVC Design Gallery accueille une collection de conceptions gratuites de sites Web qui ont été créées spécifiquement pour l’utilisation dans un projet ASP.NET MVC. Les dessins sont téléchargés par les membres de la communauté. Les visiteurs de la Galerie peuvent voter pour leurs dessins préférés (voir la figure 2).

[![Boîte de dialogue New Project](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figure 02**: The ASP.NET MVC Design Gallery ([Cliquez pour voir l’image grandeur nature](iteration-2-make-the-application-look-nice-cs/_static/image4.png))

Comme je l’écris ce tutoriel, le design le plus populaire dans la galerie est un design nommé Octobre par David Hauser. Vous pouvez utiliser cette conception pour un projet MVC ASP.NET en remplissant les étapes suivantes :

1. Cliquez sur le bouton **Télécharger** pour télécharger le fichier October.zip sur votre ordinateur.
2. Cliquez à droite sur le fichier October.zip téléchargé et cliquez sur le bouton **Débloquez** (voir la figure 3).
3. Décompressez le fichier à un dossier nommé Octobre.
4. Sélectionnez tous les fichiers du dossier DesignTemplate contenu dans le dossier d’octobre, cliquez à droite sur les fichiers et sélectionnez l’option menu **Copier**.
5. Cliquez à droite sur le nœud du projet ContactManager dans la fenêtre Visual Studio Solution Explorer et sélectionnez l’option de menu **Pâte** (voir figure 4).
6. Sélectionnez l’option de menu Visual Studio **Modifier, trouver et remplacer, remplacer rapidement** et remplacer *[MyProjectName]* par *ContactManager* (voir figure 5).

[![Boîte de dialogue New Project](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figure 03**: Débloquer un fichier téléchargé à partir du web ([Cliquez pour voir l’image grandeur nature](iteration-2-make-the-application-look-nice-cs/_static/image6.png))

[![Boîte de dialogue New Project](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figure 04**: Overwriting files in the Solution Explorer ([Cliquez pour voir l’image grandeur nature](iteration-2-make-the-application-look-nice-cs/_static/image8.png))

[![Boîte de dialogue New Project](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figure 05**: Remplacer [ProjectName] par ContactManager ([Cliquez pour voir l’image grandeur nature](iteration-2-make-the-application-look-nice-cs/_static/image10.png))

Une fois que vous avez terminé ces étapes, votre application web utilisera la nouvelle conception. La page de la figure 6 illustre l’apparence de la demande de gestionnaire de contact avec la conception d’octobre.

[![Boîte de dialogue New Project](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figure 06**: ContactManager avec le modèle d’octobre ([Cliquez pour voir l’image grandeur nature](iteration-2-make-the-application-look-nice-cs/_static/image12.png))

## <a name="creating-a-custom-aspnet-mvc-design"></a>Création d’un design MVC personnalisé ASP.NET

Le ASP.NET MVC Design Gallery a une bonne sélection de différents styles de design. La Galerie vous offre un moyen indolore de personnaliser l’apparence de vos applications ASP.NET MVC. Et, bien sûr, la Galerie a le grand avantage d’être complètement libre.

Cependant, vous pourriez avoir besoin de créer un design tout à fait unique pour votre site Web. Dans ce cas, il est logique de travailler avec une société de conception de sites Web. J’ai décidé d’adopter cette approche pour la conception de l’application Contact Manager.

J’ai zippé le gestionnaire de contact de l’itération #1 et envoyé le projet à la société de conception. Ils ne possédaient pas Visual Studio (honte sur eux!), mais cela ne présentait pas un problème. Ils ont pu télécharger Microsoft Visual Web [https://www.asp.net](https://www.asp.net) Developer gratuitement à partir du site Web et ouvrir l’application Contact Manager dans Visual Web Developer. En quelques jours, ils avaient produit la conception dans la figure 7.

[![Boîte de dialogue New Project](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figure 07**: The ASP.NET MVC Contact Manager Design ([Cliquez pour voir l’image grandeur nature](iteration-2-make-the-application-look-nice-cs/_static/image14.png))

La nouvelle conception se composait de deux fichiers principaux : un nouveau fichier de feuille de style en cascade et un nouveau fichier de page master de vue. Une page principale de vue contient la mise en page et le contenu partagé pour les vues dans une application MVC ASP.NET. Par exemple, la page principale de vue comprend l’en-tête, les onglets de navigation et le pied qui apparaissent dans la figure 7. J’ai surmené la page principale existante de Site.Master dans le dossier Views-Shared avec le nouveau fichier Site.Master de la société de conception,

La société de design a également créé une nouvelle feuille de style en cascade et un ensemble d’images. J’ai placé ces nouveaux fichiers dans le dossier content et j’ai dépassé le fichier site.css existant. Vous devez placer tout le contenu statique dans le dossier Content.

Notez que la nouvelle conception pour le gestionnaire de contact comprend des images pour l’édition et la suppression des contacts. Une image Edit et Supprimer apparaît à côté de chaque contact dans le tableau HTML des contacts.

À l’origine, ces liens qui ont été rendus avec le HTML. ActionLink() assistant comme ceci:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

La méthode Html.ActionLink() ne prend pas en charge les images (la méthode HTML code le texte du lien pour des raisons de sécurité). Par conséquent, j’ai remplacé les appels à Html.ActionLink() par des appels à Url.Action() comme ceci:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

La méthode Html.ActionLink() rend un lien hypertexte HTML entier. La méthode Url.Action(), d’autre part, rend &lt;seulement&gt; l’URL sans l’étiquette.

Notez, en outre, que la nouvelle conception comprend à la fois des onglets sélectionnés et non sélectionnés. Par exemple, dans la figure 8, l’onglet **Créer un nouveau contact** est sélectionné et l’onglet Mes **contacts** n’est pas sélectionné.

[![Boîte de dialogue New Project](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figure 08**: Onglets sélectionnés et non sélectionnés[(Cliquez pour voir l’image grandeur nature](iteration-2-make-the-application-look-nice-cs/_static/image16.png))

Pour prendre en charge le rendu des onglets sélectionnés et non sélectionnés, j’ai créé un assistant HTML personnalisé nommé menuItemHelper. Cette méthode d’aide &lt;rend&gt; soit &lt;une étiquette li&gt; ou une classe li "sélectionné" tag selon que le contrôleur actuel et l’action correspond au contrôleur et le nom d’action passé à l’aide. Le code du MenuItemHelper est contenu dans la liste 1.

**Liste 1 - Helpers-MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

Le MenuItemHelper utilise la classe TagBuilder en interne pour construire l’étiquette &lt;HTML li.&gt; La classe TagBuilder est une classe d’utilité très utile que vous pouvez utiliser chaque fois que vous avez besoin pour construire une nouvelle balise HTML. Il comprend des méthodes pour ajouter des attributs, ajouter des classes CSS, générer des ids et modifier le HTML interne de l étiquette.

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons amélioré la conception visuelle de notre application ASP.NET MVC. Tout d’abord, vous avez été présenté à la ASP.NET MVC Design Gallery. Vous avez appris à télécharger des modèles de conception gratuits à partir de la ASP.NET MVC Design Gallery que vous pouvez utiliser dans vos applications ASP.NET MVC.

Ensuite, nous avons discuté de la façon dont vous pouvez créer une conception personnalisée en modifiant le fichier de feuille de style en cascade par défaut et le fichier de page de vue maître. Afin de soutenir la nouvelle conception, nous avons dû apporter quelques modifications mineures à notre application Contact Manager. Par exemple, nous avons ajouté un nouvel assistant HTML nommé menuItemHelper qui affiche des onglets sélectionnés et non sélectionnés.

Dans la prochaine itération, nous abordons le sujet très important de validation. Nous ajoutons du code de validation à notre application afin qu’un utilisateur ne puisse pas créer un nouveau contact sans fournir les valeurs requises telles que le prénom et le nom de famille d’une personne.

> [!div class="step-by-step"]
> [Suivant précédent](iteration-1-create-the-application-cs.md)
> [Next](iteration-3-add-form-validation-cs.md)
