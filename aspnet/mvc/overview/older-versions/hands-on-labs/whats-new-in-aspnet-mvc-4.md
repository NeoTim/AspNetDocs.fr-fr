---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Quelles sont les nouveautés dans ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 est une infrastructure pour générer des applications web évolutive et basée sur des normes à l’aide de modèles de conception bien établis et la puissance de ASP.NET et...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: adb6e990d0ca98df38c47adf8c37fdffeb6d3d73
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65112998"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Nouveautés d’ASP.NET MVC 4

par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 est une infrastructure pour générer des applications web évolutive et basée sur des normes à l’aide de modèles de conception bien établis et la puissance de ASP.NET et .NET framework. Cette nouvelle, quatrième version du framework se concentre sur la simplification du développement d’applications web mobiles.

Pour commencer, lorsque vous créez un nouveau projet ASP.NET MVC 4 il existe désormais un modèle de projet d’application mobile que vous pouvez utiliser pour générer une application autonome en particulier pour les appareils mobiles. En outre, ASP.NET MVC 4 s’intègre avec jQuery Mobile via un package jQuery.Mobile.MVC NuGet. jQuery Mobile est une infrastructure de type HTML5 pour développer des applications web qui sont compatibles avec toutes les plateformes d’appareils mobiles populaires, y compris Windows Phone, iPhone, Android et ainsi de suite. Toutefois, si vous avez besoin de spécialisation, ASP.NET MVC 4 vous permet également répondre à des vues différentes pour différents appareils et fournir les optimisations spécifiques à l’appareil.

Dans cet atelier, vous allez démarrer avec ASP.NET MVC 4 &quot;Application Internet&quot; modèle de projet pour créer une application de galerie de photos. Vous allez améliorer progressivement l’application à l’aide de jQuery Mobile et les nouveautés de ASP.NET MVC 4 pour le rendre compatible avec différents appareils mobiles et les navigateurs web de bureau. Vous allez également découvrir nouvelle recettes de code pour la génération de code et comment ASP.NET MVC 4 facilite pour vous permettent d’écrire des méthodes d’action asynchrones en prenant en charge de la tâche&lt;ActionResult&gt; types de retour.

> [!NOTE]
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet spécifique à ce laboratoire est disponible à l’adresse [What ' s New in Web Forms dans ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier, vous allez apprendre comment :

- Tirer parti des améliorations apportées à l’ASP.NET MVC projet modèles, y compris le nouveau modèle de projet d’application mobile
- Utilisez l’attribut de la fenêtre d’affichage de HTML5 et les requêtes de média CSS afin d’améliorer l’affichage sur les appareils mobiles
- Utiliser jQuery Mobile pour les améliorations progressives et pour la création de l’interface utilisateur web tactile optimisée
- Créer des vues spécifiques au mobile
- Utilisez le composant de sélecteur de vue pour basculer entre les vues mobiles et de bureau dans l’application
- Créer des contrôleurs asynchrones à l’aide de la prise en charge de la tâche

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prérequis

Vous devez disposer des éléments suivants pour terminer ce laboratoire :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe B](#AppendixB) pour obtenir des instructions sur la façon d’installer).
- [ASP.NET MVC 4](../../../mvc4.md) (inclus dans l’installation de Microsoft Visual Studio 2012)
- Émulateur de Windows Phone (inclus dans le [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Facultatif : [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) avec **Electric Plum iPhone simulateur** extension (uniquement pour l’exercice 3 permet d’accéder à l’application web avec un simulateur iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Installation

Dans le document de laboratoire, vous serez invité à insérer des blocs de code. Pour votre commodité, la majeure partie de ce code est fourni en tant que Visual Studio des extraits de Code, que vous pouvez utiliser à partir de Visual Studio pour éviter d’avoir à ajouter manuellement.

Pour installer les extraits de code :

1. Ouvrez une fenêtre de l’Explorateur Windows et accédez à l’atelier **Source\Setup** dossier.
2. Double-cliquez sur le **Setup.cmd** fichier dans ce dossier pour installer les extraits de code Visual Studio.

Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [annexe a : À l’aide d’extraits de Code](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique inclut les exercices suivants :

1. [Nouveaux modèles de projet ASP.NET MVC 4](#Exercise1)
2. [Création de l’Application Web de la galerie de photos](#Exercise2)
3. [Ajout de prise en charge pour les appareils mobiles](#Exercise3)
4. [À l’aide de contrôleurs asynchrones](#Exercise4)

> [!NOTE]
> Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices. Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.

Durée estimée pour effectuer ce laboratoire : **60 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Exercice 1 : Nouveaux modèles de projet ASP.NET MVC 4

Dans cet exercice, vous allez explorer les améliorations dans les modèles de projet ASP.NET MVC 4. Outre le modèle Application Internet, déjà présents dans MVC 3, cette version inclut désormais un modèle séparé pour les applications mobiles. Tout d’abord, vous allez examiner certaines fonctionnalités pertinentes de chacun des modèles. Ensuite, vous allez travailler sur votre page correctement sur les différentes plateformes de rendu à l’aide de la bonne approche.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Tâche 1 : Explorer le modèle d’Application Internet

1. Ouvrez **Visual Studio**.
2. Sélectionnez le **fichier | Nouveau | Projet** commande de menu. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche arborescence, puis choisissez **ASP.NET MVC 4 Web Application.** Nommez le projet **PhotoGallery**, sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK**.

    > [!NOTE]
    > Vous personnaliserez ultérieurement la solution PhotoGallery ASP.NET MVC 4 que vous créez.

    ![Création d’un projet](whats-new-in-aspnet-mvc-4/_static/image1.png "création d’un projet")

    *Création d’un projet*
3. Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Application Internet** modèle de projet et cliquez sur **OK**. Assurez-vous que vous avez sélectionné Razor comme moteur d’affichage.

    ![Création d’une Application ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "création d’une Application ASP.NET MVC 4 Internet")

    *Création d’une Application ASP.NET MVC 4 Internet*

    > [!NOTE]
    > Syntaxe Razor a été introduite dans ASP.NET MVC 3. Son objectif est de réduire le nombre de caractères et séquences de touches requis dans un fichier, en activant un rapide et le fluide de flux de travail de codage. Tire parti de Razor existant C# / VB (ou autres) compétences linguistiques et offre une syntaxe de balisage de modèle qui permet un flux de travail de construction HTML exceptionnel.
4. Appuyez sur **F5** pour exécuter la solution et afficher les modèles renouvelés. Vous pouvez consulter les fonctionnalités suivantes :

    **Modèles de moderne**

    Les modèles ont été renouvelés, en fournissant plus de styles modernes.

    ![Modèles ASP.NET MVC 4 redessiné](whats-new-in-aspnet-mvc-4/_static/image3.png "adaptée des modèles de MVC 4")

    *Modèles ASP.NET MVC 4 redessiné*

    ![Nouvelle page de Contact](whats-new-in-aspnet-mvc-4/_static/image4.png "page Nouveau Contact")

    *Nouvelle page de Contact*

    **Rendu adaptatif**

    Découvrez de redimensionnement de la fenêtre du navigateur et notez la façon dont la mise en page s’adapte dynamiquement à la nouvelle taille de fenêtre. Ces modèles utilisent la technique de rendu adaptatif pour afficher correctement dans les plateformes de bureau et mobiles sans aucune personnalisation.

    ![Modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents](whats-new-in-aspnet-mvc-4/_static/image5.png "le modèle de projet ASP.NET MVC 4 dans les tailles autre navigateur")

    *Modèle de projet ASP.NET MVC 4 dans les tailles autre navigateur*

    **Interface utilisateur plus riches avec JavaScript**

    Une autre amélioration apportée aux modèles de projet par défaut est l’utilisation de JavaScript pour fournir un JavaScript plus interactive. Les liens de connexion et inscription utilisés dans le modèle illustrent comment utiliser le Validations de jQuery pour valider les champs d’entrée à partir du côté client.

    ![jQuery Validation](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery Validation*

    > [!NOTE]
    > Notez que le journal des deux sections, dans la première section, vous pouvez vous connecter à l’aide d’un compte inscrit à partir du site et dans la deuxième section, vous pouvez également vous connecter à l’aide d’un autre service d’authentification tels que google (désactivé par défaut).
5. Fermez le navigateur pour arrêter le débogueur, revenez à Visual Studio.
6. Ouvrez le fichier **AuthConfig.cs** situé sous le **application\_Démarrer** dossier.
7. Supprimez le commentaire de la dernière ligne pour inscrire le client de Google pour *OAuth* l’authentification.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Notez que vous pouvez facilement activer l’authentification à l’aide de n’importe quel service OpenID ou OAuth tels que Facebook, Twitter, Microsoft, etc.
8. Appuyez sur **F5** pour exécuter la solution et de naviguer vers la page de connexion.
9. Sélectionnez **Google** service pour vous connecter.

    ![En sélectionnant le journal de service](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *En sélectionnant le journal de service*
10. Connectez-vous en utilisant votre compte Google.
11. Autoriser le site (localhost) à extraire des informations de compte Google.
12. Enfin, vous devrez inscrire dans le site à associer le compte Google.

   ![Associer votre compte Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Associer votre compte Google*
13. Fermez le navigateur pour arrêter le débogueur, revenez à Visual Studio.
14. Explorez à présent la solution à consulter certaines nouvelles fonctionnalités introduites par ASP.NET MVC 4 dans le modèle de projet.

   ![Le modèle de projet ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image9.png "le modèle de projet Application Internet ASP.NET MVC 4")

   *Le modèle de projet Application Internet ASP.NET MVC 4*

    - **HTML 5 balisage**

       Parcourir les vues de modèle pour découvrir le nouveau balisage de thème.

       ![Nouveau modèle, à l’aide de Razor et HTML5 balisage About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "Nouveau modèle, à l’aide de Razor et HTML5 balisage About.cshtml.")

       *Nouveau modèle, en utilisant le balisage Razor et HTML5 (About.cshtml).*
    - **Bibliothèques JavaScript mis à jour**

       Le modèle par défaut de ASP.NET MVC 4 inclut désormais KnockoutJS, une infrastructure MVVM JavaScript qui vous permet de créer de riches et applications web hautement réactives à l’aide de JavaScript et HTML. Comme dans MVC 3, jQuery et bibliothèques d’interface utilisateur jQuery sont également inclus dans ASP.NET MVC 4.

     > [!NOTE]
     > Vous pouvez obtenir plus d’informations sur la bibliothèque KnockOutJS dans ce lien : [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). En outre, vous pouvez en savoir plus sur jQuery et l’interface utilisateur jQuery dans [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Tâche 2 : Explorer le modèle d’Application Mobile

ASP.NET MVC 4 facilite le développement de sites Web pour mobile et Tablet PC navigateurs. Ce modèle a la même structure d’application en tant que le modèle d’Application Internet (Notez que le code du contrôleur est pratiquement identique), mais son style a été modifié pour afficher correctement dans les appareils mobiles basés sur les fonctions tactiles.

1. Sélectionnez le **fichier | Nouveau | Projet** commande de menu. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche arborescence, puis choisissez le **ASP.NET MVC 4 Web Application.** Nommez le projet **PhotoGallery.Mobile**, sélectionnez un emplacement (ou conservez la valeur par défaut), sélectionnez &quot;ajouter à la solution&quot; et cliquez sur **OK**.
2. Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Mobile Application** modèle de projet et cliquez sur **OK**. Assurez-vous que vous avez sélectionné Razor comme moteur d’affichage.

    ![Création d’une Application ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "création d’une Application ASP.NET MVC 4 Mobile")

    *Création d’une Application ASP.NET MVC 4 Mobile*
3. Vous êtes maintenant en mesure d’Explorer la solution et découvrez quelques-unes des nouvelles fonctionnalités introduites par le modèle de solution ASP.NET MVC 4 pour mobile :

    - **jQuery Mobile bibliothèque**

        Le modèle de projet Mobile Application inclut la bibliothèque jQuery Mobile, qui est une bibliothèque open source pour la compatibilité de navigateur mobile. jQuery Mobile s’applique à l’amélioration progressive pour les navigateurs mobiles qui prennent en charge de CSS et JavaScript. Amélioration progressive permet à tous les navigateurs afficher le contenu de base d’une page web, alors que cela n’active que les navigateurs les plus puissantes afficher du contenu riche. Les fichiers JavaScript et CSS, inclus dans le jQuery Mobile style, aident les navigateurs mobiles pour ajuster le contenu à l’écran sans apporter de modification dans le balisage de page.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *bibliothèque jQuery mobile incluse dans le modèle*
    - **Balisage HTML5 en fonction**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Modèle d’application mobile à l’aide de balisage HTML5 (Login.cshtml et index.cshtml)*
4. Appuyez sur **F5** pour exécuter la solution.
5. Ouvrez le **Windows Phone 7 Emulator**.
6. Dans l’écran d’accueil de téléphone, ouvrez Internet Explorer. Vérifiez l’URL où l’application de bureau a démarré et accédez à cette URL à partir du téléphone (par exemple, `http://localhost:[PortNumber]/`).
7. Maintenant vous êtes en mesure d’entrer la page de connexion ou découvrir le sur la page. Notez que le style du site Web est basé sur la nouvelle application de Metro pour mobile. Le modèle de projet ASP.NET MVC 4 s’affiche correctement sur les appareils mobiles, s’assurer que tous les éléments de la page sont visibles et activés. Notez que les liens de l’en-tête sont assez grand pour être cliqué ou appuyé.

    ![Projet de pages de modèle dans un appareil mobile](whats-new-in-aspnet-mvc-4/_static/image14.png "projet des pages de modèle dans un appareil mobile")

    *Pages de modèle de projet dans un appareil mobile*
8. Le nouveau modèle utilise également le **balise meta de la fenêtre d’affichage**. Les navigateurs mobiles plus définissent une largeur d’une fenêtre de navigateur virtuel ou &quot;viewport&quot;, qui est supérieur à la largeur réelle de l’appareil mobile. Cela permet aux navigateurs mobiles afficher la page web entière à l’intérieur de l’écran virtuel. Le **balise meta de la fenêtre d’affichage** permet aux développeurs web définir la largeur, hauteur et la mise à l’échelle de la zone de navigateur sur les appareils mobiles **.** Le modèle ASP.NET MVC 4 pour les Applications mobiles définit la fenêtre d’affichage de la largeur de l’appareil (&quot;largeur = largeur de l’appareil&quot;) dans le modèle de disposition (*Views\Shared\_Layout.cshtml*), de sorte que tous les pages verront leur fenêtre d’affichage à passer à la largeur d’écran de périphérique. Notez que la balise meta de la fenêtre d’affichage ne change pas la vue du navigateur par défaut.
9. Ouvrez  **\_Layout.cshtml**, situé dans le **vues | Partagé** dossier et le commentaire de la balise méta du Viewport. Exécutez l’application, si ce n’est pas déjà ouvert et découvrez les différences.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Le site après la balise méta viewport de commentaires](whats-new-in-aspnet-mvc-4/_static/image15.png "le site après la balise méta viewport de commentaires")

*Le site après la balise méta viewport de commentaires*
10. Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.
11. Supprimez les commentaires de la balise méta viewport.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Tâche 3 : à l’aide de rendu adaptatif

Dans cette tâche, vous allez apprendre une autre méthode pour restituer une page Web correctement sur les appareils mobiles et les navigateurs Web en même temps sans aucune personnalisation. Vous avez déjà utilisé la balise meta de la fenêtre d’affichage avec un objectif similaire. Maintenant vous répondra à une autre méthode puissante : *rendu adaptatif*.

Rendu adaptatif est une technique qui utilise **les requêtes de média CSS3** pour personnaliser le style appliqué à une page. Les requêtes de média définissent les conditions à l’intérieur d’une feuille de style, en regroupant les styles CSS sous une certaine condition. Uniquement lorsque la condition est true, le style est appliqué aux objets déclarés.

La flexibilité fournie par la technique de rendu adaptatif permet toute personnalisation pour afficher le site sur différents appareils. Vous pouvez définir des styles autant que vous le souhaitez sur une feuille de style unique sans écrire de code de logique pour choisir le style. Par conséquent, il est un moyen très pratique d’adaptation des styles de page, car cela réduit la quantité de code dupliqué et logique pour à des fins de rendu. En revanche, la consommation de bande passante augmenterait, comme la taille de vos fichiers CSS peut légèrement augmenter.

À l’aide de la technique de rendu adaptatif, votre site sera **affiché correctement, quel que soit le navigateur.** Toutefois, vous devez envisager si le charge de la bande passante supplémentaire est un problème.

> [!NOTE]
> Le format de base d’une requête de média est : @media \[Étendue : tous les | poche | imprimer | projection | écran\] ([ : valeur de propriété] et... propriété : valeur)

Exemples de requêtes de média : &gt;  **@media tous les et (la largeur maximale : 1000px) et (la largeur minimale : 700px) {}:** Pour toutes les résolutions entre 700px et 1000px.

> **@media écran et (la largeur minimale : 400px) et (la largeur maximale : 700px) {...} :** Uniquement pour les écrans. La résolution doit être compris entre 400 et 700px.
> 
> **@media ordinateur de poche et (la largeur minimale : 20em), écran et (la largeur minimale : 20em) {...} :** Pour les ordinateurs de poche (mobile et appareils) et les écrans. La largeur minimale doit être supérieure à 20em.
> 
> Vous trouverez plus d’informations sur la [W3C site](http://www.w3.org/TR/css3-mediaqueries/).

Vous allez maintenant Explorer le fonctionnement de rendu adaptable, améliorer la lisibilité de ASP.NET MVC 4 par défaut le modèle de site Web.

1. Ouvrez le **PhotoGallery.sln** solution que vous avez créé à la tâche 1 et sélectionnez le **PhotoGallery** projet. Appuyez sur **F5** pour exécuter la solution.
2. Redimensionner la largeur du navigateur, définissant les fenêtres à la moitié ou au moins un quart de sa taille d’origine. Notez que se passe-t-il avec les éléments dans l’en-tête : Certains éléments n’apparaîtront pas dans la zone visible de l’en-tête.
3. Ouvrez **Site.css** fichier à partir de l’Explorateur de Solution Visual Studio, situé dans **contenu** dossier du projet. Appuyez sur **CTRL + F** pour ouvrir recherche intégrée de Visual Studio et écrire **@media** pour localiser le **requête de média CSS**.

    La condition de requête de média définie dans ce modèle fonctionne de cette façon : Lorsque la taille de la fenêtre du navigateur est ci-dessous **850 px**, les règles CSS appliquées sont celles définies à l’intérieur de ce bloc de média.

    ![Localisation de la requête de média](whats-new-in-aspnet-mvc-4/_static/image16.png "recherche de la requête de média")

    *Localisation de la requête de média*
4. Remplacez la valeur d’attribut de la largeur maximale de 850 px avec **10px**, afin de désactiver le rendu adaptatif, puis appuyez sur **CTRL + S** pour enregistrer les modifications. Retourner au navigateur et appuyez sur **CTRL + F5** pour actualiser la page avec les modifications que vous avez apportées. Notez les différences dans les deux pages lors de l’ajustement de la largeur de la fenêtre.

    ![Dans la gauche, la page est mise la @media style, dans la droite, le style est omis](whats-new-in-aspnet-mvc-4/_static/image17.png "gauche, la page demande le @media style, dans la droite, le style est omis")

    *Gauche, la page demande le @media style, dans la droite, le style est omis.*

    À présent, penchons-nous sur ce qui se passe sur les appareils mobiles :

    ![Dans la gauche, la page est mise la @media style, dans la droite, le style est omis](whats-new-in-aspnet-mvc-4/_static/image18.png "gauche, la page demande le @media style, dans la droite, le style est omis")

    *Gauche, la page demande le @media style, dans la droite, le style est omis.*

    Bien que vous pouvez remarquer que les modifications lorsque la page est affichée dans un navigateur Web ne sont pas très importantes, lors de l’utilisation d’un appareil mobile, les différences deviennent plus évidents. Sur le côté gauche de l’image, nous pouvons voir que le style personnalisé amélioré la lisibilité.

    Rendu adaptatif peut être utilisé dans de nombreux scénarios de plus, ce qui facilite l’appliquer conditionnel d’un style à un site Web et de résolution des problèmes courants de style avec une approche intéressante.

    La balise meta de la fenêtre d’affichage et les requêtes de média CSS ne sont pas spécifiques à ASP.NET MVC 4, afin de tirer parti de ces fonctionnalités dans n’importe quelle application web.
5. Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Exercice 2 : Création de l’Application Web de la galerie de photos

Dans cet exercice, vous allez travailler sur une application de galerie de photos pour afficher les photos. Vous allez commencer avec le modèle de projet ASP.NET MVC 4, et ensuite, vous ajoutez une fonctionnalité pour récupérer des photos à partir d’un service et les afficher dans la page d’accueil.

Dans l’exercice suivant, vous mettrez à jour cette solution pour améliorer la manière dont il est affiché sur des appareils mobiles.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Tâche 1 : création d’un Service factice Photo

Dans cette tâche, vous allez créer un simulacre du service pour récupérer le contenu qui s’affichera dans la galerie de photos. Pour ce faire, vous allez ajouter un nouveau contrôleur qui retourne simplement un fichier JSON avec les données de chaque photo.

1. Ouvrez **Visual Studio** si pas déjà ouvert.
2. Sélectionnez le **fichier | Nouveau | Projet** commande de menu. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche arborescence, puis choisissez **ASP.NET MVC 4 Web Application.** Nommez le projet **PhotoGallery**, sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK**. Ou bien, vous pouvez continuer à travailler à partir de votre existante ASP.NET MVC 4 **Application Internet** solution à partir de **exercice 1** et ignorez l’étape suivante.
3. Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Application Internet** modèle de projet et cliquez sur **OK**. Assurez-vous que vous avez sélectionné en tant que le moteur d’affichage de Razor.
4. Dans le **l’Explorateur de solutions**, avec le bouton droit le **application\_données** dossier de votre projet, puis sélectionnez **ajouter | Un élément existant**. Accédez à la **Source\Assets\App\_données** dossier de ce laboratoire et ajoutez le **Photos.json** fichier.
5. Créer un nouveau contrôleur portant le nom **PhotoController**. Pour ce faire, cliquez sur le **contrôleurs** dossier, accédez à **ajouter** et sélectionnez **contrôleur.** Complétez le nom de contrôleur, laissez le **contrôleur MVC vide** modèle et cliquez sur **ajouter**.

    ![Ajout de la PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "ajoutant le PhotoController")

    *Ajout de la PhotoController*
6. Remplacez le **Index** méthode par le code suivant **galerie** action et retournent le contenu à partir du fichier JSON que vous avez récemment ajouté au projet.

    (Code Snippet - *Action ASP.NET MVC 4 de la galerie de Lab - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Appuyez sur **F5** et exécuter la solution, puis accédez à l’URL suivante pour tester le service factice photo : `http://localhost:[port]/photo/gallery` (dépend de la valeur [port] sur le port actuel où l’application a été lancée). La demande à cette URL doit récupérer le contenu de la **Photos.json** fichier.

    ![Test du service de photo factices](whats-new-in-aspnet-mvc-4/_static/image20.png "test du service de photo simulée")

    *Test du service de photo simulée*

Dans une implémentation réelle, vous pouvez utiliser [API Web ASP.NET](../../../../web-api/index.md) pour implémenter le service de la galerie de photos. API Web ASP.NET est une infrastructure qui facilite la création de services HTTP qui atteignent une large gamme de clients, y compris les navigateurs et appareils mobiles. L'API Web ASP.NET est une plate-forme idéale pour générer des applications RESTful sur le .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Tâche 2 - affichage de la galerie de photos

Dans cette tâche, vous mettrez à jour la page d’accueil pour afficher la galerie de photos en utilisant le service factice que vous avez créé dans la première tâche de cet exercice. Ajouter des fichiers de modèle et à mettre à jour les vues de la galerie.

1. Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.
2. Créer le **Photo** classe dans le **modèles** dossier. Pour ce faire, cliquez sur le **modèles** dossier, sélectionnez **ajouter** et cliquez sur **classe**. Ensuite, définissez le nom sur **Photo.cs** et cliquez sur **ajouter**.
3. Ajoutez les membres suivants à la **Photo** classe.

    (Code Snippet - *modèle de Photo d’ASP.NET MVC 4 Lab - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Ouvrez le fichier **HomeController.cs** dans le dossier **Controllers**.
5. Ajoutez les instructions using suivantes.

    (Code Snippet - *ASP.NET MVC 4 les instructions Using HomeController Lab - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Mise à jour le **Index** action à utiliser **HttpClient** pour récupérer les données de la galerie, puis utiliser le **JavaScriptSerializer** à désérialiser au modèle de vue.

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Ouvrez le **Index.cshtml** fichier situé dans le **Views\Home** dossier et remplacez tout le contenu par le code suivant.

    Ce code effectue une itération sur toutes les photos extraites du service et les affiche dans une liste non triée.

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. Dans le **l’Explorateur de solutions**, avec le bouton droit le **contenu** dossier de votre projet, puis sélectionnez **ajouter | Un élément existant**. Accédez à la **Source\Assets\Content** dossier de ce laboratoire et ajoutez le **Site.css** fichier. Vous devrez confirmer son remplacement. Si vous avez le **Site.css** le fichier est ouvert, vous devrez confirmer pour recharger le fichier également.
9. Ouvrez l’Explorateur de fichiers et de copier l’intégralité de **Photos** dossier situé sous le **Source\Assets** dossier de ce laboratoire dans le dossier racine de votre projet dans l’Explorateur de solutions.
10. Exécutez l'application. Vous devez maintenant voir la page d’accueil affichant les photos dans la galerie.

    ![Galerie de photos](whats-new-in-aspnet-mvc-4/_static/image21.png "galerie de photos")

    *Galerie de photos*
11. Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Exercice 3 : Ajout de prise en charge pour les appareils mobiles

Les mises à jour de clés dans ASP.NET MVC 4 est la prise en charge pour le développement mobile. Dans cet exercice, vous allez explorer les nouvelles fonctionnalités de ASP.NET MVC 4 pour les applications mobiles en étendant la solution de galerie photo que vous avez créé dans l’exercice précédent.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Tâche 1 : installation jQuery Mobile dans une Application ASP.NET MVC 4

1. Ouvrez le **commencer** solution situé dans **/Ex3-MobileSupport/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez le **Console du Gestionnaire de Package** en cliquant sur le **outils** > **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**  option de menu.

    ![Ouverture de la Console du Gestionnaire de Package NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "ouverture de la Console du Gestionnaire de Package NuGet")

    *Ouverture de la Console de gestionnaire de Package NuGet*
3. Dans la Console du Gestionnaire de Package, exécutez la commande suivante pour installer le **jQuery.Mobile.MVC** package.

    jQuery Mobile est une bibliothèque open source pour la création de l’interface utilisateur web tactile optimisée. Le package jQuery.Mobile.MVC NuGet inclut des programmes d’assistance pour utiliser jQuery Mobile avec une application ASP.NET MVC 4.

    > [!NOTE]
    > En exécutant la commande suivante, vous télécharge la bibliothèque jQuery.Mobile.MVC à partir de Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Cette commande installe jQuery Mobile et des fichiers d’assistance, notamment les suivantes :

    - **Views/Shared/\_Layout.Mobile.cshtml**: est une disposition jQuery Mobile optimisée pour un écran plus petit. Lorsque le site Web reçoit une demande à partir d’un navigateur mobile, il remplacera la disposition d’origine (\_Layout.cshtml) avec celui-ci.
    - Un composant de sélecteur de vue : se compose de la **Views/Shared/\_ViewSwitcher.cshtml** vue partielle et le **ViewSwitcherController.cs** contrôleur. Ce composant affiche un lien sur les navigateurs mobiles pour permettre aux utilisateurs de basculer vers la version bureau de la page.  
        ![Projet de la galerie de photos avec prise en charge mobile](whats-new-in-aspnet-mvc-4/_static/image23.png "projet de la galerie de photos avec prise en charge mobile")

        *Projet de la galerie de photos avec prise en charge mobile*
4. Inscrire les regroupements Mobile. Pour ce faire, ouvrez le **Global.asax.cs** fichier, puis ajoutez la ligne suivante.

    (Code Snippet - *ASP.NET MVC 4 Mobile offres groupées de Registre Lab - Ex03 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Exécutez l’application à l’aide d’un navigateur web de bureau.
6. Ouvrez le **émulateur de Windows Phone 7,** situé dans **Menu Démarrer | Tous les programmes | Windows Phone SDK 7.1 | Émulateur de Windows Phone.**
7. Dans l’écran d’accueil de téléphone, ouvrez Internet Explorer. Vérifiez l’URL où l’application a démarré et accédez à cette URL avec le navigateur de téléphone (par exemple, `http://localhost:[PortNumber]/`).

    Vous remarquerez que votre application aura un aspect différente dans l’émulateur Windows Phone, comme le jQuery.Mobile.MVC a créé de nouvelles ressources dans votre projet qui montrent les vues optimisées pour les appareils mobiles.

    Notez que le message en haut du téléphone, montrant le lien qui bascule vers l’affichage de bureau. En outre, le  **\_Layout.Mobile.cshtml** mise en page qui a été créé par le package que vous avez installé, y compris une autre disposition dans l’application.

    > [!NOTE]
    > Jusqu’ici, il n’existe aucun lien pour revenir à l’affichage mobile. Il est inclus dans les versions ultérieures.

    ![Affichage mobile de la page d’accueil de la galerie Photo](whats-new-in-aspnet-mvc-4/_static/image24.png "affichage Mobile de la page d’accueil de la galerie Photo")

    *Affichage mobile de la page d’accueil de la galerie Photo*
8. Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Tâche 2 : création de vues mobiles

Dans cette tâche, vous allez créer une version mobile de la vue index avec un contenu adapté pour une meilleure apparence dans les appareils mobiles.

1. Copie le **Views\Home\Index.cshtml** afficher et la coller pour créer une copie, renommez le nouveau fichier à **Index.Mobile.cshtml**.
2. Ouvrez le nouveau créé **Index.Mobile.cshtml** permet d’afficher et de remplacer la &lt;ul&gt; balise avec ce code. Ce faisant, vous a mis à jour le &lt;ul&gt; balise jQuery Mobile et annotations de données à utiliser les thèmes mobiles à partir de jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Notez que :
    > 
    > - Le **data-role** attribut la valeur **listview** affichera la liste en utilisant les styles de listview.
    > 
    > - Le **incrustation de données** attribut défini sur true affiche la liste avec coins arrondis et marge.
    > 
    > - Le **filtre de données** attribut la valeur **true** génère une zone de recherche.
    > 
    > Pour plus d’informations sur les conventions de jQuery Mobile dans la documentation du projet : [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Appuyez sur **CTRL + S** pour enregistrer les modifications.
4. Basculez vers le **Windows Phone Emulator** et actualiser le site. Notez que la nouvelle apparence de la liste de la galerie, ainsi que la nouvelle zone de recherche située en haut. Ensuite, tapez un mot dans la zone de recherche (par exemple, **Tulips**) pour démarrer une recherche dans la galerie de photos.

    ![Galerie à l’aide du style de listview avec filtrage](whats-new-in-aspnet-mvc-4/_static/image25.png "galerie à l’aide du style de listview avec filtrage")

    *Galerie à l’aide du style de listview avec filtrage*

    Pour résumer, vous avez utilisé la recette encourage à vue se pour créer une copie de la vue Index avec la &quot;mobile&quot; suffixe. Ce suffixe indique à ASP.NET MVC 4 que chaque demande généré à partir d’un appareil mobile utilise cette copie de l’index. En outre, vous avez mis à jour la version mobile de la vue de l’Index à utiliser jQuery Mobile permettant d’améliorer l’apparence site dans les appareils mobiles.
5. Revenez à Visual Studio et ouvrez **Site.Mobile.css** situé sous le **contenu** dossier.
6. Corrigez le positionnement du titre photo pour qu’il s’affiche sur le côté droit de l’image. Pour ce faire, ajoutez le code suivant à la **Site.Mobile.css** fichier.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Appuyez sur **CTRL + S** pour enregistrer les modifications.
8. Revenez à la **Windows Phone Emulator** et actualiser le site. Notez que le titre de la photo est convenablement positionné maintenant.

    ![Titre positionné sur le côté droit de l’image](whats-new-in-aspnet-mvc-4/_static/image26.png "titre positionné sur le côté droit de l’image")

    *Titre positionné sur le côté droit de l’image*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Tâche 3 : jQuery Mobile thèmes

Chaque mise en page et du widget dans jQuery Mobile sont conçu autour d’une infrastructure nouvelle CSS et orienté objet qui rend possible d’appliquer un thème de conception visuelle d’unifiée complète aux sites et applications.

Thème par défaut jQuery Mobile inclut 5 échantillons qui reçoivent des lettres (, b, c, d, e) pour référence rapide.

Dans cette tâche, vous mettrez à jour la disposition mobile pour utiliser un autre thème que celui par défaut.

1. Revenez à Visual Studio.
2. Ouvrez le  **\_Layout.Mobile.cshtml** fichier situé dans **Views\Shared**.
3. Rechercher l’élément div avec le rôle de données la valeur &quot;page&quot; et mettre à jour le **data-theme** à &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Appuyez sur **CTRL + S** pour enregistrer les modifications.
5. Actualiser le site dans le **Windows Phone Emulator** et notez le nouveau jeu de couleurs.

    ![Disposition Mobile avec un autre jeu de couleurs](whats-new-in-aspnet-mvc-4/_static/image27.png "disposition Mobile avec un autre jeu de couleurs")

    *Disposition Mobile avec un autre jeu de couleurs*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Tâche 4 - utilisant le composant de sélecteur de vue et le navigateur de substitution de fonctionnalités

Une convention pour les pages web optimisés pour mobile consiste à ajouter un lien dont le texte est quelque chose comme vue bureau ou le mode de site complète qui permet aux utilisateurs de basculer vers une version de bureau de la page. Le package jQuery.Mobile.MVC inclut un exemple **sélecteur de vue** composant à cet effet utilisé dans le  **\_Layout.Mobile.cshtml** vue.

![Lien pour basculer vers l’affichage du bureau](whats-new-in-aspnet-mvc-4/_static/image28.png "lien pour basculer vers l’affichage du bureau")

*Lien pour basculer vers l’affichage du bureau*

Le sélecteur de vue utilise une nouvelle fonctionnalité appelée **substitution de navigateur**. Cette fonctionnalité permet à votre application de traiter les demandes comme si elles provenaient d’un autre navigateur (agent utilisateur) que celui qu’ils proviennent réellement.

Dans cette tâche, vous allez explorer l’exemple d’implémentation d’un sélecteur de vue ajouté par jQuery.Mobile.MVC et le nouveau navigateur substitution de fonctionnalités dans ASP.NET MVC 4.

1. Revenez à Visual Studio.
2. Ouvrez le  **\_Layout.Mobile.cshtml** vue situé sous le **Views\Shared** dossier et notez que le composant de sélecteur de vue référencé par une vue partielle.

    ![Disposition Mobile à l’aide du composant de sélecteur de vue](whats-new-in-aspnet-mvc-4/_static/image29.png "disposition Mobile à l’aide du composant de sélecteur de vue")

    *Disposition Mobile à l’aide du composant de sélecteur de vue*
3. Ouvrez le  **\_ViewSwitcher.cshtml** vue partielle.

    La vue partielle utilise la nouvelle méthode **ViewContext.HttpContext.GetOverriddenBrowser()** pour déterminer l’origine de la demande web et afficher le lien correspondant pour basculer une pour les vues de bureau ou Mobile.

    Le **GetOverriddenBrowser** méthode retourne un **HttpBrowserCapabilitiesBase** instance qui correspond à l’agent utilisateur actuellement défini pour la demande (réelle ou substituée). Vous pouvez utiliser cette valeur pour obtenir des propriétés telles que **IsMobileDevice**.

    ![Vue partielle ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "vue partielle ViewSwitcher")

    *Vue partielle ViewSwitcher*
4. Ouvrez le **ViewSwitcherController.cs** classe situé dans le **contrôleurs** dossier. Consultez cette action SwitchView est appelée par le lien dans le composant ViewSwitcher et notez les nouvelles méthodes HttpContext.

    - Le **HttpContext.ClearOverriddenBrowser()** méthode supprime tout agent utilisateur substitué pour la requête actuelle.
    - Le **HttpContext.SetOverriddenBrowser()** méthode remplace la valeur de l’agent utilisateur réelle de la demande à l’aide de l’agent utilisateur spécifié.  
        ![Contrôleur de ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher contrôleur")  
*ViewSwitcher contrôleur*

        Substitution de navigateur est une fonctionnalité principale d’ASP.NET MVC 4, qui est également disponible même si vous n’installez pas le package jQuery.Mobile.MVC. Toutefois, cette fonctionnalité affecte uniquement affichage, la disposition et vue partielle, et il n’affecte pas les fonctionnalités qui dépendent de l’objet Request.Browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Tâche 5 : ajout du sélecteur de vue dans l’affichage de bureau

Dans cette tâche, vous mettrez à jour le mode Poste de travail afin d’inclure le sélecteur de vue. Cela permettra aux utilisateurs mobiles de revenir en arrière à la vue mobile lorsque vous parcourez l’affichage de bureau.

1. Actualiser le site dans le **Windows Phone Emulator**.
2. Cliquez sur le **vue bureau** lien en haut de la galerie. Vous remarquez aucun sélecteur de vue dans l’affichage de bureau pour permettre de que revenir à l’affichage mobile.
3. Revenez à Visual Studio et ouvrez le  **\_Layout.cshtml** vue.
4. Recherchez la section de connexion et insérez un appel à restituer le  **\_ViewSwitcher** vue partielle ci-dessous le  **\_LogOnPartial** vue partielle. Ensuite, appuyez sur **CTRL + S** pour enregistrer les modifications.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Appuyez sur **CTRL + S** pour enregistrer les modifications.
6. Actualisez la page dans l’émulateur Windows Phone et double-cliquez sur l’écran pour effectuer un zoom. Notez que la page d’accueil affiche maintenant le **affichage Mobile** lien bascule mobile en mode bureau.

    ![Afficher le sélecteur de rendu en mode Bureau](whats-new-in-aspnet-mvc-4/_static/image32.png "basculeur restitué en mode bureau")

    *Sélecteur de vue restituée dans la vue bureau*
7. Basculez vers l’affichage Mobile à nouveau et accédez à **sur** page (http://localhost[port] / Home/sur). Notez que, même si vous n’avez pas créé une vue About.Mobile.cshtml, la page About est affichée à l’aide de la disposition mobile (\_Layout.Mobile.cshtml).

    ![Sur la page](whats-new-in-aspnet-mvc-4/_static/image33.png "sur la page")

    *Sur la page*
8. Enfin, ouvrez le site dans un navigateur Web de bureau. Notez qu’aucune des précédentes mises à jour a affecté l’affichage de bureau.

    ![Vue bureau PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "vue bureau PhotoGallery")

    *Vue bureau PhotoGallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Tâche 6 : création de nouveaux Modes d’affichage

La nouvelle fonctionnalité de modes d’affichage permet à une application de sélectionner les vues selon le navigateur qui génère la requête. Par exemple, si un navigateur de bureau demande la page d’accueil, l’application retournera le **Views\Home\Index.cshtml** modèle. Ensuite, si un navigateur mobile demande la page d’accueil, l’application retournera le **Views\Home\Index.mobile.cshtml** modèle.

Dans cette tâche, vous allez créer une disposition personnalisée pour les appareils iPhone, et vous ne devrez pas simuler des requêtes à partir d’appareils iPhone. Pour ce faire, vous pouvez utiliser soit un émulateur/simulateur iPhone (comme [électrique simulateur Mobile](http://www.electricplum.com/)) ou un navigateur avec des modules complémentaires qui modifient l’agent utilisateur. Pour obtenir des instructions sur la façon de définir la chaîne d’agent utilisateur dans un navigateur Safari émuler un iPhone, consultez [comment permettre à Safari prétendre qu’il est IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) dans les blog de David Alison.

**Notez que cette tâche est facultative et vous pouvez continuer tout au long de l’atelier sans l’exécuter.**

1. Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.
2. Ouvrez **Global.asax.cs** et ajoutez le code suivant à l’aide d’instruction.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Ajoutez le code en surbrillance suivant à l’Application\_Start (méthode).

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Vous avez inscrit un nouveau **DefaultDisplayMode nommé &quot;iPhone&quot;**, au sein de la méthode statique **DisplayModeProvider.Instance.Modes** liste statique, qui sera comparée à chaque requête entrante. Si la demande entrante contient la chaîne &quot;iPhone&quot;, ASP.NET MVC trouveront les vues dont le nom contient le &quot;iPhone&quot; suffixe. Le paramètre 0 indique la façon dont certains est le nouveau mode ; par exemple, cette vue est plus spécifique que le général &quot;.mobile&quot; règle qui correspond à des demandes à partir d’appareils mobiles.

Une fois ce code s’exécute lorsqu’un navigateur iPhone génère une demande, votre application utilisera le **Views\Shared\\_Layout.iPhone.cshtml** disposition que vous allez créer dans les étapes suivantes.

> [!NOTE]
> Cette méthode de test de la demande pour iPhone a été simplifié à des fins de démonstration et peut ne pas fonctionne comme prévu pour chaque chaîne d’agent utilisateur iPhone (pour le test de l’exemple respecte la casse).

4. Créer une copie de la  **\_Layout.Mobile.cshtml** de fichiers dans le **Views\Shared** dossier et renommez la copie à &quot; **\_Layout.iPhone.cshtml**&quot;.
5. Ouvrez  **\_Layout.iPhone.cshtml** vous avez créé à l’étape précédente.
6. Rechercher l’élément div avec l’attribut data-role à **page** et modifiez le **data-theme** attribut &quot; **un**&quot;.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Maintenant, vous avez 3 mises en page dans votre application ASP.NET MVC 4 :

1. **\_Layout.cshtml**: présentation par défaut utilisée pour les navigateurs de bureau.
2. **\_Layout.Mobile.cshtml**: présentation par défaut utilisée pour les appareils mobiles.
3. **\_Layout.iPhone.cshtml**: disposition spécifique pour les appareils iPhone, à l’aide d’un jeu de couleurs pour différencier les \_Layout.mobile.cshtml.
7. Appuyez sur **F5** pour exécuter l’application et de parcourir le site dans le **Windows Phone Emulator**.
8. Ouvrir un **simulateur iPhone** (consultez [annexe C](#AppendixC) pour obtenir des instructions sur la façon d’installer et configurer un simulateur iPhone) et accédez au site trop. Notez que chaque téléphone utilise le modèle spécifique.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *À l’aide des vues différentes pour chaque appareil mobile*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Exercice 4 : À l’aide de contrôleurs asynchrones

Microsoft .NET Framework 4.5 introduit de nouvelles fonctionnalités de langage dans c# et Visual Basic pour fournir une nouvelle fondation pour le comportement asynchrone dans la programmation .NET. Cette nouvelle foundation rend la programmation asynchrone similaire à - et aussi simple que la programmation synchrone. Vous êtes maintenant en mesure d’écrire des méthodes d’action asynchrones dans ASP.NET MVC 4 à l’aide de la **AsyncController** classe. Vous pouvez utiliser les méthodes d’action asynchrones pour la durée d’exécution longue, pas le processeur lié des demandes. Cela évite de bloquer le serveur Web de réaliser un travail pendant le traitement de la demande. La classe AsyncController est généralement utilisée pour les appels de service Web long terme.

Cet exercice explique les principes fondamentaux de l’opération asynchrone dans ASP.NET MVC 4. Si vous souhaitez approfondir vos connaissances, vous pouvez consulter l’article suivant : [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Tâche 1 : implémentation d’un contrôleur asynchrone

1. Ouvrez le **commencer** solution situé dans **/Ex4-Async/début du fichierSource/** dossier. Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez le **HomeController.cs** classe à partir de la **contrôleurs** dossier.
3. Ajoutez le code suivant à l’aide d’instruction.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Mise à jour le **HomeController** classe hérite de **AsyncController**. Les contrôleurs qui dérivent de AsyncController permettent à ASP.NET de traiter des requêtes asynchrones, et ils peuvent toujours les méthodes d’action synchrones service.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Ajouter le **async** mot clé pour le **Index** (méthode) et définissez-la comme le type de retour **tâche&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > Le **async** mot clé est un des nouveaux mots clés fournit le .NET Framework 4.5 ; elle indique au compilateur que cette méthode contient du code asynchrone. Un **tâche** objet représente une opération asynchrone qui peut se terminer à un moment donné dans le futur.
6. Remplacez le **client. GetAsync()** appel avec la version complète async en utilisant le mot clé await comme indiqué ci-dessous.

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > Dans la version précédente, que vous utilisiez le **résultat** propriété à partir de la **tâche** objet pour bloquer le thread jusqu'à ce que le résultat est retourné (version de synchronisation).
    > 
    > Ajout de la **await** mot clé indique au compilateur d’attendre la tâche retournée à partir de l’appel de méthode de façon asynchrone. Cela signifie que le reste du code sera exécuté comme un rappel uniquement après que la méthode attendue est terminée. Une autre chose à remarquer est que vous n’avez pas besoin de modifier votre bloc try-catch afin d’y parvenir : les exceptions qui se produisent en arrière-plan ou en premier plan seront toujours être interceptées sans aucun travail supplémentaire à l’aide d’un gestionnaire fourni par l’infrastructure.
7. Modifier le code pour continuer avec l’implémentation asynchrone, en remplaçant les lignes avec le nouveau code, comme indiqué ci-dessous

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Exécutez l'application. Vous ne remarquerez aucune modification majeure, mais votre code ne bloquera pas d’un thread du pool de threads apporter une meilleure utilisation des ressources du serveur et améliore les performances.

    > [!NOTE]
    > Vous trouverez plus d’informations sur les nouvelles fonctionnalités de programmation asynchrones dans le laboratoire &quot; **la programmation asynchrone dans .NET 4.5 avec c# et Visual Basic** &quot; inclus dans le Kit de formation Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Tâche 2 : délais d’expiration de la gestion des jetons d’annulation

Méthodes d’action asynchrones qui retournent des instances de tâche peuvent prennent également en charge les délais d’attente. Dans cette tâche, vous mettrez à jour le code de la méthode Index pour gérer un scénario de délai d’attente à l’aide d’un jeton d’annulation.

1. Revenez à Visual Studio, puis appuyez sur **MAJ + F5** pour arrêter le débogage.
2. Ajoutez le code suivant à l’aide de l’instruction à la **HomeController.cs** fichier.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Mettre à jour l’action de l’Index à recevoir un **CancellationToken** argument.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Mise à jour le **GetAsync** appel pour passer le jeton d’annulation.

    (Code Snippet - *SendAsync de Lab - Ex04 - ASP.NET MVC 4 avec CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Décorer le *Index* méthode avec un **AsyncTimeout** attribut défini sur 500 millisecondes et un **HandleError** attribut configurée pour traiter les  **TaskCanceledException** en redirigeant vers un **TimedOut** vue.

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - attributs*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Ouvrir le **PhotoController** classe et mise à jour le **galerie** méthode pour différer les millisecondes de l’exécution de 1000 (1 seconde) pour simuler une tâche à long terme.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Ouvrez le **Web.config** de fichiers et d’activer les erreurs personnalisées en ajoutant l’élément suivant.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Créer une nouvelle vue dans **Views\Shared** nommé **TimedOut** et utiliser la disposition par défaut. Dans l’Explorateur de solutions, cliquez sur le **Views\Shared** dossier et sélectionnez **ajouter | Vue**.

    ![À l’aide des vues différentes pour chaque appareil mobile](whats-new-in-aspnet-mvc-4/_static/image36.png "à l’aide des vues différentes pour chaque appareil mobile")

    *À l’aide des vues différentes pour chaque appareil mobile*
9. Mise à jour le **TimedOut** afficher le contenu comme indiqué ci-dessous.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Exécutez l’application et accédez à l’URL racine. Comme vous avez ajouté un **Thread.Sleep** de 1 000 millisecondes, vous obtiendrez une erreur de délai d’attente, générée par le **AsyncTimeout** d’attribut et d’intercepter par le **HandleError** attribut.

    ![Exception de délai d’attente de traitement](whats-new-in-aspnet-mvc-4/_static/image37.png "exception de délai d’attente de traitement")

    *Exception de délai d’attente de traitement*

> [!NOTE]
> En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [annexe d : Publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixD).

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Cette ateliers pratiques, vous avez observé certaines des nouvelles fonctionnalités qui se trouvent dans ASP.NET MVC 4. Les concepts suivants ont été abordés :

- Tirer parti des améliorations apportées à l’ASP.NET MVC projet modèles, y compris le nouveau modèle de projet d’application mobile
- Utilisez l’attribut de la fenêtre d’affichage de HTML5 et les requêtes de média CSS afin d’améliorer l’affichage sur les appareils mobiles
- Utiliser jQuery Mobile pour les améliorations progressives et pour la création de l’interface utilisateur web tactile optimisée
- Créer des vues spécifiques au mobile
- Utilisez le composant de sélecteur de vue pour basculer entre les vues mobiles et de bureau dans l’application
- Créer des contrôleurs asynchrones à l’aide de la prise en charge de la tâche

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Annexe a : À l’aide d’extraits de Code

Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main. Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.

![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](whats-new-in-aspnet-mvc-4/_static/image38.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")

*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***

1. Placez le curseur où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).
3. Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).
5. Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](whats-new-in-aspnet-mvc-4/_static/image39.png "commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](whats-new-in-aspnet-mvc-4/_static/image40.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*

![Appuyez sur Tab à nouveau et l’extrait de code seront développe](whats-new-in-aspnet-mvc-4/_static/image41.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")

*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*

***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)***

1. Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.
2. Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.
3. Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.

![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](whats-new-in-aspnet-mvc-4/_static/image42.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")

*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*

![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](whats-new-in-aspnet-mvc-4/_static/image43.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")

*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Annexe b : Installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; *Visual Studio Express 2012 pour le Web avec Windows Azure SDK*&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l'installation](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation est terminée](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.

    ![VS Express pour une vignette de Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express pour une vignette de Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Annexe c : L’installation de WebMatrix 2 et iPhone simulateur

Pour exécuter votre site dans un appareil simulé iPhone, vous pouvez utiliser l’extension de WebMatrix &quot;électrique simulateur Mobile pour l’iPhone&quot;. En outre, vous pouvez configurer la même extension pour exécuter le simulateur à partir de Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Tâche 1 : installation de WebMatrix 2

1. Accédez à [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; *WebMatrix 2*&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "installer WebMatrix 2")

    *Installer WebMatrix 2*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](whats-new-in-aspnet-mvc-4/_static/image50.png "acceptant les termes du contrat de licence")

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l’installation](whats-new-in-aspnet-mvc-4/_static/image51.png "progression de l’Installation")

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![L’installation est terminée](whats-new-in-aspnet-mvc-4/_static/image52.png "l’Installation est terminée")

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Tâche 2 : installation de l’Extension de simulateur iPhone

1. Exécutez **WebMatrix** et ouvrir n’importe quel site Web existant ou créez-en un.
2. Cliquez sur le **exécuter** bouton à partir de la **accueil** ruban et sélectionnez **Ajouter nouveau**.

    ![Ajout d’une nouvelle extension de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "Ajout d’une nouvelle extension de WebMatrix")

    *Ajout d’une nouvelle extension de WebMatrix*
3. Sélectionnez **iPhone simulateur** et cliquez sur **installer**.

    ![Exploration des extensions de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "extensions de navigation de WebMatrix")

    *Exploration des extensions de WebMatrix*
4. Dans les détails du package, cliquez sur **installer** pour poursuivre l’installation de l’extension.

    ![extension de simulateur d’iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone extension du simulateur")

    *extension de simulateur d’iPhone*
5. Lisez et acceptez la CLUF de l’extension.

    ![Extension de WebMatrix CLUF](whats-new-in-aspnet-mvc-4/_static/image56.png "extension WebMatrix CLUF")

    *Extension de WebMatrix CLUF*
6. Maintenant, vous pouvez exécuter votre site Web à partir de WebMatrix à l’aide de l’option de simulateur iPhone.

    ![Exécuter à l’aide d’iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "exécuter à l’aide d’iPhone")

    *Exécuter à l’aide d’iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Tâche 3 : configuration de Visual Studio 2012 pour exécuter iPhone simulateur

1. Ouvrez **Visual Studio 2012** et ouvrir n’importe quel site Web ou créez un nouveau projet.
2. Cliquez sur la flèche vers le bas à partir du bouton d’exécution et sélectionnez **naviguer avec**.

    ![Naviguer avec](whats-new-in-aspnet-mvc-4/_static/image58.png "naviguer avec")

    *Naviguer avec*
3. Dans le &quot;naviguer avec&quot; boîte de dialogue, cliquez sur **ajouter**.
4. Dans le &quot;ajouter un programme&quot; boîte de dialogue, utilisez les valeurs suivantes :

   - **Programme**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(le chemin de mise à jour en conséquence)*
   - **Arguments** : &quot;1&quot;
   - **Nom convivial**: iPhone simulateur

     ![Ajouter un programme](whats-new-in-aspnet-mvc-4/_static/image59.png "ajouter un programme")

     *Ajouter un programme à naviguer avec*
5. Cliquez sur **OK** et fermer les boîtes de dialogue.
6. Vous êtes maintenant en mesure d’exécuter vos applications Web dans le simulateur iPhone à partir de Visual Studio 2012.

    ![Naviguer avec iPhone simulateur](whats-new-in-aspnet-mvc-4/_static/image60.png "naviguer avec iPhone simulateur")

    *Naviguer avec iPhone simulateur*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe d : Publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail de gestion Windows Azure et publiez l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tâche 1 : création d’un nouveau Site Web à partir de Windows Azure Portal

1. Accédez à la [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Windows Azure, vous pouvez héberger 10 Sites Web ASP.NET gratuitement et faites ensuite évoluer que votre trafic augmente. Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).

    ![Ouvrez une session sur le portail Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "ouvrez une session sur le portail Windows Azure")

    *Ouvrez une session sur le portail de gestion Azure Windows*
2. Cliquez sur **New** sur la barre de commandes.

    ![Création d’un Site Web](whats-new-in-aspnet-mvc-4/_static/image62.png "création d’un Site Web")

    *Création d’un Site Web*
3. Cliquez sur **calcul** | **Site Web**. Puis sélectionnez **création rapide** option. Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.

    > [!NOTE]
    > Un Site Web Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer. L’option Création rapide vous permet de déployer une application web terminée pour le Site Web Windows Azure à partir en dehors du portail. Il n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un Site Web à l’aide de la création rapide](whats-new-in-aspnet-mvc-4/_static/image63.png "création d’un Site Web à l’aide de la création rapide")

    *Création d’un Site Web à l’aide de la création rapide*
4. Attendez que la nouvelle **Site Web** est créé.
5. Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne. Vérifiez que le nouveau Site Web fonctionne.

    ![Navigation vers le nouveau site web](whats-new-in-aspnet-mvc-4/_static/image64.png "exploration vers le nouveau site web")

    *Navigation vers le nouveau site web*

    ![Site Web en cours d’exécution](whats-new-in-aspnet-mvc-4/_static/image65.png "site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.

    ![Ouvrir les pages de gestion de site web](whats-new-in-aspnet-mvc-4/_static/image66.png "ouvrir les pages de gestion de site web")

    *Ouvrir les pages de gestion de Site Web*
7. Dans le **tableau de bord** page, sous la **aperçu rapide** , cliquez sur le **télécharger le profil de publication** lien.

    > [!NOTE]
    > Le *le profil de publication* contient toutes les informations requises pour publier une application web à un site Web Windows Azure pour chaque méthode de publication est activée. Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour la connexion et l’authentification par rapport à chacun des points de terminaison pour lequel une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge de la lecture des profils pour automatiser la configuration de ces programmes pour de publication publication d’applications web sur sites Web Windows Azure.

    ![Téléchargement du site web le profil de publication](whats-new-in-aspnet-mvc-4/_static/image67.png "téléchargement du site web le profil de publication")

    *Téléchargement du Site Web le profil de publication*
8. Télécharger le fichier de profil de publication dans un emplacement connu. Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.

    ![L’enregistrement du fichier de profil de publication](whats-new-in-aspnet-mvc-4/_static/image68.png "l’enregistrement du profil de publication")

    *L’enregistrement du fichier de profil de publication*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données. Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.

1. Vous devez un serveur de base de données SQL pour stocker la base de données d’application. Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à l’adresse **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**. Si vous n’avez pas un serveur créé, vous pouvez créer un à l’aide du **ajouter** bouton sur la barre de commandes. Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et mot de passe**, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore, la base de données tel qu’il sera créé dans une étape ultérieure.

    ![Tableau de bord de serveur SQL Database](whats-new-in-aspnet-mvc-4/_static/image69.png "tableau de bord serveur de base de données SQL")

    *Tableau de bord serveur de base de données SQL*
2. Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP cliente actuelle** et collez-le sur le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) bouton.

    ![Ajout d’adresse IP du Client](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Ajout d’adresse IP du Client*
3. Une fois le **adresse IP du Client** est ajouté à le des adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confirmer les modifications*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.

    ![Publication de l’Application](whats-new-in-aspnet-mvc-4/_static/image73.png "publication de l’Application")

    *Publier le site web*
2. Importer le profil de publication que vous avez enregistré dans la première tâche.

    ![L’importation du profil de publication](whats-new-in-aspnet-mvc-4/_static/image74.png "l’importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la Validation terminée. Cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.

    ![Validation de la connexion](whats-new-in-aspnet-mvc-4/_static/image75.png "validation de la connexion")

    *Validation de la connexion*
4. Dans le **paramètres** page, sous la **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).

    ![Configuration de déploiement Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configuration de déploiement Web")

    *Configuration de déploiement Web*
5. Configurez la connexion de base de données comme suit :

   - Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.
   - Dans **nom d’utilisateur** tapez votre nom de connexion d’administrateur serveur.
   - Dans **mot de passe** tapez votre mot de passe du compte de connexion administrateur serveur.
   - Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.

     ![Configuration de chaîne de connexion de destination](whats-new-in-aspnet-mvc-4/_static/image77.png "configuration de chaîne de connexion de destination")

     *Configuration de chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](whats-new-in-aspnet-mvc-4/_static/image78.png "création de la chaîne de la base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous utiliserez pour vous connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte de la connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers la base de données SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "chaîne de connexion pointant vers la base de données SQL")

    *Chaîne de connexion pointant vers la base de données SQL*
8. Dans le **aperçu** , cliquez sur **publier**.

    ![Publication de l’application web](whats-new-in-aspnet-mvc-4/_static/image80.png "publication de l’application web")

    *Publication de l’application web*
9. Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.

    ![Application publiée dans Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application publiée dans Windows Azure")

    *Application soit publiée dans Windows Azure*
