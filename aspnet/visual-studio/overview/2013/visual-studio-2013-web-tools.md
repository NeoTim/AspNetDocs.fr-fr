---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Atelier pratique : Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: Visual Studio est un environnement de développement excellente pour. Projets web et Windows en fonction du NET. Il inclut un éditeur de texte puissant qui peut facilement être utilisé pour...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 82248efd767c1110b9a4067b7d0c0e2ecafcbef9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049886"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Atelier pratique : Visual Studio 2013 Web Tools
====================
par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](http://aka.ms/webcamps-training-kit)

> Visual Studio est un environnement de développement excellente pour. Projets web et Windows en fonction du NET. Il inclut un éditeur de texte puissant qui peut facilement être utilisé pour modifier les fichiers autonomes sans projet.
> 
> Visual Studio gère une arborescence d’analyse complète lorsque vous modifiez chaque fichier. Cela permet à Visual Studio fournir inégalées la saisie semi-automatique et les actions basées sur le document tout en rendant l’expérience de développement plus rapide et plus agréable. Ces fonctionnalités sont particulièrement efficace dans les documents HTML et CSS.
> 
> Toute cette puissance est également disponible pour les extensions, rendant simple à étendre les éditeurs de nouvelles fonctionnalités puissantes pour répondre à vos besoins. Web Essentials est une collection de (principalement) les améliorations liées au web pour Visual Studio. Il inclut un grand nombre de nouveau saisies semi-automatiques IntelliSense (en particulier pour CSS), les nouvelles fonctionnalités de lien du navigateur, automatique des fichiers JSHint pour JavaScript, nouveaux avertissements pour HTML et CSS et de nombreuses autres fonctionnalités qui sont essentielles au développement web moderne.
> 
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Vue d'ensemble

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier, vous allez apprendre comment :

- Utiliser les nouvelles fonctionnalités de l’éditeur HTML incluses dans Web Essentials, tels que des extraits de code HTML5 riches et Zen codage
- Utiliser les nouvelles fonctionnalités de l’éditeur CSS incluses dans Web Essentials telles que le sélecteur de couleurs et une info-bulle de la matrice navigateur
- Utiliser les nouvelles fonctionnalités de l’éditeur JavaScript incluses dans Web Essentials telles qu’extraction au fichier et IntelliSense pour tous les éléments HTML
- Échanger des données entre votre navigateur et de Visual Studio à l’aide du lien du navigateur

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prérequis

Les éléments suivants sont nécessaire pour terminer ce laboratoire pratique :

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou supérieur
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Installation

Afin d’exécuter les exercices dans cet atelier, vous devez configurer votre environnement tout d’abord.

1. Ouvrez une fenêtre de l’Explorateur Windows et accédez à l’atelier **Source** dossier.
2. Avec le bouton droit **Setup.cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui configure votre environnement et installer les extraits de code Visual Studio pour ce laboratoire.
3. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.

> [!NOTE]
> Assurez-vous que vous avez activé toutes les dépendances pour ce laboratoire avant d’exécuter le programme d’installation.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>À l’aide d’extraits de Code

Dans le document de laboratoire, vous serez invité à insérer des blocs de code. Pour votre commodité, la majeure partie de ce code est fourni en tant que Visual Studio extraits de Code, vous pouvez y accéder à partir de Visual Studio 2013 pour éviter d’avoir à ajouter manuellement.

> [!NOTE]
> Chaque exercice est accompagnée d’une solution de départ située dans le **commencer** dossier de l’exercice qui vous permet de suivre chaque exercice indépendamment des autres. N’oubliez pas que les extraits de code sont ajoutés au cours d’un exercice sont manquants à partir de ces solutions de démarrage et peut ne pas fonctionnent jusqu'à ce que vous avez terminé l’exercice. Dans le code source pour un exercice, vous y trouverez également un **fin** dossier qui contient une solution Visual Studio avec le code qui résulte d’effectuer les étapes dans l’exercice correspondant. Si vous avez besoin d’aide au cours de cet atelier, vous pouvez utiliser ces solutions en tant que guide.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique inclut les exercices suivants :

1. [Utilisation de lien du navigateur et Web Essentials](#Exercise1)
2. [En tirant parti des extraits de Code IntelliSense](#Exercise2)

> [!NOTE]
> Lorsque vous démarrez Visual Studio, vous devez sélectionner une des collections de paramètres prédéfinis. Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, extraits de code IntelliSense et les options de boîte de dialogue. Les procédures décrites dans ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lorsque vous utilisez le **paramètres de développement généraux** collection. Si vous choisissez une collection de paramètres différents pour votre environnement de développement, il peut y avoir des différences dans les étapes que vous devez prendre en compte.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Exercice 1 : Utilisation de lien du navigateur et Web Essentials

**Web Essentials** est une extension Visual Studio qui ajoute de nombreuses fonctionnalités utiles pour le développement web moderne, principalement pour but de rendre l’expérience de développement web beaucoup plus rapide et plus agréable. Vous pouvez installer Web Essentials à partir de la galerie d’extensions dans Visual Studio.

**Lien du navigateur** est une nouvelle fonctionnalité incluse dans Visual Studio 2013 qui fournit un canal entre l’IDE Visual Studio et n’importe quel navigateur ouvert pour échanger des données entre votre application web et de Visual Studio. Web Essentials étend le lien du navigateur avec des outils pour manipuler le modèle d’objet DOM et les styles CSS de vos pages web directement à partir du navigateur.

Dans cet exercice, vous allez explorer certaines des fonctionnalités prises en charge par **Web Essentials** et **Browser Link** pour améliorer une page simple questionnaire.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Tâche 1 : le projet en cours d’exécution dans plusieurs navigateurs

Dans cette tâche, vous allez configurer votre application web à exécuter dans plusieurs navigateurs à la fois, ce qui est utile pour les tests entre navigateurs.

1. Ouvrez **Microsoft Visual Studio**.
2. Dans le **fichier** menu, sélectionnez **Open | Projet/Solution...**  et accédez à **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** dans le **Source** dossier du laboratoire (C:\WebCampsTK\HOL\VSWebTooling\Source). Sélectionnez **Begin.sln** et cliquez sur **Open**.
3. Dans la barre d’outils Visual Studio, développez le menu navigateur et sélectionnez **naviguer avec...** .

    ![Naviguer avec l’option de menu](visual-studio-2013-web-tools/_static/image1.png "naviguer avec... dans le menu de l’Explorateur")

    *Naviguer avec l’option de menu*
4. Dans le **naviguer avec** boîte de dialogue, sélectionnez **Google Chrome** et **Internet Explorer** en maintenant enfoncée la **CTRL** clé et cliquez sur  **Définir comme valeur par défaut**.

    ![Naviguer avec la boîte de dialogue](visual-studio-2013-web-tools/_static/image2.png "naviguer avec la boîte de dialogue")

    *Sélection de plusieurs navigateurs par défaut*
5. Google Chrome et Internet Explorer doivent maintenant apparaître en tant que les navigateurs par défaut. Cliquez sur **Annuler** pour fermer la boîte de dialogue.

    ![Google Chrome et Internet Explorer comme navigateurs par défaut](visual-studio-2013-web-tools/_static/image3.png "Internet Explorer et Google Chrome navigateurs par défaut")

    *Google Chrome et Internet Explorer comme navigateurs par défaut*

    > [!NOTE]
    > Après avoir configuré les navigateurs par défaut, le **plusieurs navigateurs** option est sélectionnée dans le menu du navigateur.
    > 
    > ![Plusieurs navigateurs](visual-studio-2013-web-tools/_static/image4.png "plusieurs navigateurs")
6. Appuyez sur **CTRL** + **F5** pour exécuter l’application sans débogage.
7. Lorsque les deux fenêtres de navigateur s’ouvrent, placez un d’eux au-dessus de l’autre afin de voir les mises à jour sur les deux navigateurs simultanément. Les navigateurs doivent afficher une page web avec un rectangle bleu clair.

    ![Placer un seul navigateur au-dessus de l’autre](visual-studio-2013-web-tools/_static/image5.png "placer un seul navigateur au-dessus de l’autre")

    *Placer un seul navigateur au-dessus de l’autre*
8. Ne fermez pas les navigateurs. Vous les utiliserez dans la tâche suivante.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Tâche 2 : Zen à l’aide de codage pour créer des éléments HTML

**Codage Zen** est un éditeur de codage de plug-in pour haut débit HTML, XML, XSL (ou tout autre format de code structuré) et la modification. Le cœur de ce plug-in est un moteur d’abréviation puissant qui vous permet de développer des expressions - semblables aux sélecteurs CSS - code HTML. Codage Zen est un moyen rapide d’écrire la syntaxe du sélecteur de style HTML à l’aide de CSS.

Dans cet exercice, vous utiliserez la fonctionnalité de codage Zen fournie par Web Essentials pour générer les boutons HTML qui représentent les options de la question.

1. Revenez à Visual Studio.
2. Ouvrez le **Index.cshtml** fichier situé dans le **vues** | **accueil** dossier.
3. Remplacez le **&lt;!--TODO : ajoutez ici--options&gt;** commentaire avec le code suivant, puis appuyez sur **onglet**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Le code doit être développé au format HTML.

    ![Développé HTML](visual-studio-2013-web-tools/_static/image6.png "développé HTML")

    *HTML développé*

    > [!NOTE]
    > Pour en savoir plus sur la syntaxe de codage Zen, consultez la rubrique suivante [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Cliquez sur le **actualiser les navigateurs liés** bouton Mettre à jour les deux navigateurs.

    ![Actualiser les navigateurs liés](visual-studio-2013-web-tools/_static/image7.png "actualiser les navigateurs liés")

    *Actualiser les navigateurs liés*

    ![Internet Explorer - Page mise à jour avec quatre boutons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page mise à jour avec quatre boutons")

    *Internet Explorer - Page mise à jour avec quatre boutons*

    ![Google Chrome - Page mise à jour avec quatre boutons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page mise à jour avec quatre boutons")

    *Google Chrome - Page mise à jour avec quatre boutons*
6. Revenez à Visual Studio.
7. Vous avez ajouté les boutons à la page, mais vous devez toujours ajouter une question de modèle. Pour ce faire, vous allez utiliser une nouvelle fonctionnalité dans Web Essentials appelé **Lorem Ipsum Générateur**. Recherchez le **div** élément avec la **classe** attribut **front**.
8. Ajoutez le code suivant en tant que le premier élément enfant de le **div**, puis appuyez sur **onglet**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Le code doit être développé au format HTML.

    ![Généré automatiquement Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "généré automatiquement Lorem Ipsum")

    *Généré automatiquement Lorem Ipsum*

    > [!NOTE]
    > Dans le cadre du codage Zen, vous pouvez maintenant générer Lorem Ipsum code directement dans l’éditeur HTML. Tapez simplement **lorem** puis appuyez sur **onglet** et un 30 word Lorem Ipsum texte sera inséré. Par exemple, *lorem10* insère 10 mots Lorem Ipsum.
10. Vous allez ajouter un logo en haut de la question à l’aide d’une autre nouvelle fonctionnalité dans Web Essentials appelé **Générateur de Pixel de Lorem**. Ajoutez le code suivant en tant que le premier élément enfant de le **div** élément avec **conteneur** comme **classe** valeur, puis appuyez sur **onglet**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Le code doit se développer au format HTML.

    ![Généré automatiquement Lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "généré automatiquement Lorem Pixel")

    *Généré automatiquement Lorem Pixel*

    > [!NOTE]
    > Dans le cadre du codage Zen, vous pouvez également générer le code de Pixel de Lorem directement dans l’éditeur HTML. Tapez simplement **pix-200 x 200-animaux** puis appuyez sur **onglet** et un **img** balise avec une image de 200 x 200 d’un animal sera insérée. Pour plus d’informations, consultez [Lorem Pixel](http://www.lorempixel.com).
12. Cliquez sur le **actualiser les navigateurs liés** bouton Mettre à jour les deux navigateurs.

    ![Internet Explorer - généré automatiquement image et texte](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - généré automatiquement image et texte")

    *Internet Explorer - généré automatiquement image et texte*

    ![Google Chrome - généré automatiquement image et texte](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - généré automatiquement image et texte")

    *Google Chrome - généré automatiquement image et texte*

    > [!NOTE]
    > Étant donné que l’image est sélectionné au hasard lorsque vous ajoutez l’extrait de code, l’image affichée dans les navigateurs peut-être différer.
13. Ne fermez pas les navigateurs. Vous les utiliserez dans la tâche suivante.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Tâche 3 : mise à jour d’une propriété de Style

Dans cette tâche, vous allez utiliser le lien de navigateur **Mode inspecter** fonctionnalité pour détecter l’emplacement exact où l’élément DOM spécifique est généré et puis mettez à jour la propriété de couleur de l’élément à l’aide d’un sélecteur de couleurs fourni par le Web Essentials.

1. Dans le navigateur Internet Explorer, appuyez sur **CTRL** + **ALT** + **je** pour activer le Mode inspecter.
2. Déplacez le pointeur sur la bordure bleu claire et cliquez sur.

    ![Déplacement du pointeur sur la bordure bleu claire](visual-studio-2013-web-tools/_static/image14.png "déplacement du pointeur sur la bordure bleu claire")

    *Déplacement du pointeur sur la bordure bleu claire*
3. Revenez à Visual Studio. Notez comment l’élément HTML que vous avez sélectionné dans le navigateur est également sélectionné dans l’éditeur HTML de Visual Studio.

    ![Élément HTML sélectionné dans l’éditeur HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "élément HTML sélectionné dans l’éditeur HTML de Visual Studio")

    *Élément HTML sélectionné dans l’éditeur HTML de Visual Studio*
4. Vous met alors à jour la **front** classe CSS pour modifier le style de l’élément sélectionné. Pour ce faire, appuyez sur **CTRL** + **,** pour ouvrir le **naviguer vers** zone de recherche. Type **site.css** et appuyez sur **entrée** pour ouvrir le fichier.

    ![Ouverture du fichier Site.css](visual-studio-2013-web-tools/_static/image16.png "ouverture du fichier Site.css")

    *Ouverture du fichier Site.css*
5. Appuyez sur **CTRL** + **F** et type **.flip-Container .front** pour trouver le sélecteur CSS.
6. Cliquez sur le carré bleu clair dans la propriété de la bordure de la classe pour ouvrir le sélecteur de couleurs.

    ![Ouvrir le sélecteur de couleurs](visual-studio-2013-web-tools/_static/image17.png "ouvrir le sélecteur de couleurs")

    *Ouvrir le sélecteur de couleurs*
7. Développez le sélecteur de couleurs en cliquant sur le bouton chevron et sélectionnez une nouvelle couleur.

    ![Développez le sélecteur de couleurs](visual-studio-2013-web-tools/_static/image18.png "développant le sélecteur de couleurs")

    *Développez le sélecteur de couleurs*
8. Appuyez sur **CTRL** + **ALT** + **entrée** pour actualiser les navigateurs liés.
9. Basculez vers Internet Explorer et notez la façon dont la couleur de la bordure a changé.

    ![Internet Explorer - mise à jour de couleur de bordure](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - mise à jour de couleur de bordure")

    *Internet Explorer - mise à jour de couleur de bordure*
10. Basculez vers Google Chrome et notez la façon dont la couleur de la bordure a changé.

    ![Google Chrome - mise à jour de couleur de bordure](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - couleur de bordure mis à jour")

    *Google Chrome - couleur de bordure mis à jour*
11. Revenez à Visual Studio.
12. Accédez à la fin de la **Site.css** fichier, puis appuyez sur **CTRL** + **F** pour localiser le **.btn** sélecteur.
13. Notez que le **- webkit-border-radius** propriété est soulignée en vert.

    ![propriété - webkit-border-radius du sélecteur btn](visual-studio-2013-web-tools/_static/image21.png "propriété - webkit-border-radius du sélecteur btn")

    *propriété - webkit-border-radius du sélecteur btn*
14. Placer le signe insertion dans la **- webkit-border-radius** propriété. Une ligne bleue doit apparaître sous la première lettre du premier mot de la propriété. Il s’agit du **balise active**.
15. Appuyez sur **CTRL** + **.** Pour ouvrir le menu des suggestions et cliquez sur **ajouter la propriété standard (border-radius) manquante**.

    ![Ajouter manquant suggestion de propriété standard](visual-studio-2013-web-tools/_static/image22.png "ajouter manquant suggestion de propriété standard")

    *Ajouter manquant suggestion de propriété standard*
16. Le **border-radius** propriété est automatiquement ajoutée à la règle CSS.

    ![Propriété standard ajoutée manquante](visual-studio-2013-web-tools/_static/image23.png "standard ajoutée la propriété manquante")

    *Propriété standard ajoutée manquante*
17. Déplacez le pointeur sur le **border-radius** propriété pour afficher le **info-bulle de la matrice navigateur**. Le **info-bulle de la matrice navigateur** indique la disponibilité de la propriété dans chaque navigateur.

    ![Info-bulle de la matrice navigateur](visual-studio-2013-web-tools/_static/image24.png "info-bulle de la matrice navigateur")

    *Info-bulle de la matrice navigateur*
18. Notez que la valeur de la **border-radius** propriété est toujours soulignée. Déplacez le pointeur sur la valeur pour afficher le message d’avertissement.

    ![Avertissement de valeur de propriété border-radius](visual-studio-2013-web-tools/_static/image25.png "avertissement de valeur de propriété Border-radius")

    *Avertissement de valeur de propriété border-radius*
19. Supprimer l’unité de la **border-radius** valeur de propriété comme indiqué par l’info-bulle.
20. En tant que **border-radius** est la propriété standard permettant de définir comment coins arrondis sont à angles, vous pouvez supprimer le **- webkit-border-radius** propriété et la valeur de la règle CSS.
21. Placer le signe insertion dans la **retour** propriété et notez que la balise active apparaît également sous.
22. Ouvrez le menu et cliquez sur **ajouter les spécificités de fournisseur manquant**.

    ![Ajouter la suggestion de spécificités fournisseur manquant](visual-studio-2013-web-tools/_static/image26.png "ajoutez suggestion de spécificités fournisseur manquant")

    *Ajouter la suggestion de spécificités fournisseur manquant*
23. Le **-ms-retour** propriété est automatiquement ajoutée à la règle CSS.

    ![Propriété spécifique de fournisseur ajoutée](visual-studio-2013-web-tools/_static/image27.png "propriété spécifique de fournisseur ajoutée")

    *Propriété spécifique de fournisseur ajoutée*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Tâche 4 - mise à jour le Code HTML à partir du navigateur

Dans cette tâche, vous allez utiliser le lien de navigateur **Mode Création** fonctionnalité pour modifier l’objet DOM à partir du navigateur et de transférer les modifications effectuées dans le fichier source HTML dans Visual Studio.

1. Google Chrome, appuyez sur **CTRL** + **ALT** + **D** pour activer le Mode de conception.
2. Déplacez le pointeur sur le **Lorem Ipsum dolor sit amet** étiqueter et cliquez sur.

    ![Modification de la question](visual-studio-2013-web-tools/_static/image28.png "modification de la question")

    *Modification de la question*
3. Un curseur doit apparaître. Remplacez le texte d’origine par *à quoi il ressemble lorsque j’écris une question de plus de temps ?*, puis appuyez sur **ÉCHAP** pour quitter le Mode de conception.

    ![Question modifiée](visual-studio-2013-web-tools/_static/image29.png "Question modifiée")

    *Question modifiée*
4. Commutateur vers Visual Studio et ouvrez **Index.cshtml**, s’il n’est déjà ouvert. Notez que le texte interne de la **&lt;p&gt;** élément a été mis à jour.

    ![Question de mise à jour dans la page HTML](visual-studio-2013-web-tools/_static/image30.png "question mis à jour dans la page HTML")

    *Question mis à jour dans la page HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Avertissements liés à la tâche 5 : examen des moteurs de recherche

**L’optimisation des moteurs de recherche** (SEO) est le processus de fabrication d’un site Web sont classés sur la liste d’un moteur de recherche de résultats. Plus le site évalue et plus cohérente, il est répertorié, les visiteurs de plus que le site obtiendra à partir de ce moteur de recherche. Web Essentials intègre un outil analytique qui examine le HTML, les rapports les problèmes trouvés et fournit une assistance pour les corriger.

1. Accédez à la **vue** menu et cliquez sur **liste d’erreurs** pour ouvrir le **liste d’erreurs** fenêtre.

    ![Liste d’erreurs dans la vue menu](visual-studio-2013-web-tools/_static/image31.png "liste d’erreurs dans le menu Affichage")

    *Liste d’erreurs dans la vue menu*
2. Notez qu’il existe un avertissement SEO qui notifier un **&lt;meta&gt;** balise pour la description de la page est manquante. Double-cliquez sur l’entrée d’avertissement SEO pour corriger ce problème.

    ![Fenêtre liste d’erreurs](visual-studio-2013-web-tools/_static/image32.png "fenêtre liste d’erreurs")

    *Fenêtre Liste d’erreurs*
3. Dans le **Web Essentials** boîte de dialogue, cliquez sur **Oui** pour insérer une description &lt;meta&gt; balise.

    ![Boîte de dialogue Web Essentials](visual-studio-2013-web-tools/_static/image33.png "boîte de dialogue Web Essentials")

    *Boîte de dialogue Web Essentials*
4. L’éditeur pour  **\_Layout.cshtml** s’ouvre et la **&lt;meta&gt;** balise est ajoutée automatiquement à la **head** section de la Fichier HTML.

    ![Balise META automatiquement ajouté dans la page de _Layout](visual-studio-2013-web-tools/_static/image34.png "balise Meta automatiquement ajouté dans la page d’une _disposition")

    *Balise META automatiquement ajouté à \_page de disposition*
5. Modifiez la valeur de la **contenu** attribut *GeekQuiz* et enregistrez le fichier.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Exercice 2 : En tirant parti des extraits de Code IntelliSense

Avec Web Essentials, l’éditeur HTML a été étendu avec des fonctionnalités supplémentaires. Dans cet exercice, vous verrez certaines nouvelles fonctionnalités qui sont utiles lors du développement d’applications web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Tâche 1 : utilisation d’IntelliSense dans des Documents HTML

La première nouvelle fonctionnalité que vous voyez dans cette tâche est appelée **IntelliSense dynamique**. IntelliSense dynamique lit les autres balises et attributs à déduire les ID possibles, que vous allez utiliser.

Dans cette tâche, vous allez créer un nouvel élément de formulaire HTML qui contient une étiquette et un champ d’entrée. Puis vous ajouterez un **pour** d’attribut de l’étiquette à lier à l’entrée, et vous verrez des suggestions IntelliSense basée sur les ID des entrées dans la portée.

1. Ouvrez **Visual Studio Express 2013 pour le Web** et le **Begin.sln** solution situé dans le **/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/début du fichier Source** dossier. Ou bien, vous pouvez continuer avec la solution que vous avez obtenue dans l’exercice précédent.
2. Dans **l’Explorateur de solutions**, ouvrez le **Index.cshtml** fichier situé dans le **vues** | **accueil** dossier.
3. Ajoutez un formulaire à l’intérieur de la **&lt;section&gt;** élément.

    (Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *formulaire*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. La balise d’entrée doit être précédée d’une étiquette avec une description du champ. Ajoutez l’étiquette suivante avant la balise d’entrée.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. Le **pour** attribut d’un **&lt;étiquette&gt;** indique à quel élément de formulaire une étiquette est liée. Valeur de l’attribut doit être égale à l’id de l’élément associé. Ajouter le **pour** attribut le **&lt;étiquette&gt;** élément. Comme indiqué dans l’illustration suivante, le &quot;nom&quot; valeur s’affiche dans la zone IntelliSense, basé sur l’id des éléments dans la même portée (englobant  **&lt;formulaire&gt;**).

    ![Indiquer l’id dans IntelliSense](visual-studio-2013-web-tools/_static/image35.png "montrant l’id dans IntelliSense")

    *Indiquer l’id dans IntelliSense*
6. Supprimer ajoutées récemment **&lt;formulaire&gt;** élément et son contenu.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Tâche 2 - extraits de Code HTML à l’aide de

HTML5 a introduit plus de 25 nouvelles balises sémantiques. Visual Studio disposait déjà prise en charge IntelliSense pour ces balises, mais Visual Studio 2013 rend plus rapide et plus facile d’écrire le balisage en ajoutant de nouveaux extraits de code. Bien que ces balises ne sont pas complexes, elles présentent quelques subtilités, telles que l’ajout les solutions de secours de codec approprié pour le *audio* balise. Dans cette tâche, vous verrez les extraits de code HTML pour la balise audio.

1. Dans le **Index.cshtml** de fichier, tapez  **&lt;aud** à l’intérieur de la **&lt;section&gt;** élément, comme indiqué dans l’illustration suivante.

    ![Insertion d’un élément audio](visual-studio-2013-web-tools/_static/image36.png "insertion d’un élément audio")

    *Insertion d’un élément audio*
2. Appuyez sur **onglet** à deux reprises et notez la façon dont le code suivant est ajouté dans la page et le curseur est placé sur le **src** attribut de la première source.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > En appuyant sur la **onglet** clé à deux reprises, l’extrait de code est inséré. L’extrait de code audio illustre l’utilisation standard de la *audio* balise, avec deux fichiers sources pour la prise en charge améliorée.
3. Supprimez la deuxième ligne et de mettre à jour de la source de la première ligne avec le lien suivant pour l’afficher WebCampsTV Katana : [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Le code résultant est indiqué ci-dessous.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Le fichier source *KatanaProject.mp3* est utilisé comme exemple. Vous pouvez utiliser une autre source si vous préférez.
4. Appuyez sur **CTRL** + **S** pour enregistrer le fichier.
5. Appuyez sur **CTRL** + **F5** pour démarrer l’application.
6. Notez qu’un lecteur audio a été ajouté à l’application.

    ![Lecteur audio dans Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player dans Internet Explorer")

    *Lecteur audio dans Internet Explorer*

    ![Lecteur audio dans Google Chrome](visual-studio-2013-web-tools/_static/image38.png "lecteur Audio dans Google Chrome")

    *Lecteur audio dans Google Chrome*
7. Ne fermez pas les navigateurs. Vous les utiliserez dans la tâche suivante.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Tâche 3 : utilisation d’IntelliSense dans les Documents de JavaScript

Avec Web Essentials 2013, feuilles de style et pages HTML génère la liste des ID et le nom de classe. Dans cette tâche, vous allez apprendre comment ces listes améliorent la prise en charge de JavaScript IntelliSense dans Web Essentials 2013.

1. Dans le **Index.cshtml** , ajoutez le code suivant pour définir un **script** balise pour le code JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Ajoutez le code suivant à l’intérieur de la **script** balise pour définir la fonction de rappel prêt.

    (Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Placer le signe insertion dans la **script** balise, puis appuyez sur **CTRL** + **.** Pour ouvrir le menu de suggestion.
4. Cliquez sur **extraire vers le fichier**.

    ![JavaScript extraire vers la suggestion de fichier](visual-studio-2013-web-tools/_static/image39.png "JavaScript extraire vers la suggestion de fichier")

    *JavaScript extraire vers la suggestion de fichier*
5. Dans le **enregistrer en tant que** fenêtre, sélectionnez le **Scripts** dossier, nommez le fichier **init.js** et cliquez sur **enregistrer**.

    ![Enregistrer en tant que fenêtre](visual-studio-2013-web-tools/_static/image40.png "enregistrer en tant que fenêtre")

    *Enregistrer en tant que fenêtre*

    > [!NOTE]
    > Le **init.js** fichier est créé et le contenu du script est déplacé vers le fichier.
    > 
    > ![Fichier init.js créé avec le contenu inclus](visual-studio-2013-web-tools/_static/image41.png "fichier Init.js créé avec le contenu inclus")
    > 
    > *Fichier init.js créé avec le contenu inclus*
6. Ouvrez le **Index.cshtml** de fichiers et vérifiez que la balise de script a été remplacée par une référence à la **init.js** fichier.

    ![Référence de html init.js](visual-studio-2013-web-tools/_static/image42.png "référence de html Init.js")

    *Référence de html init.js*
7. Accédez à la **l’Explorateur de solutions** et notez que le **init.js** fichier a été automatiquement inclus dans la solution.

    ![Fichier init.js inclus dans la solution](visual-studio-2013-web-tools/_static/image43.png "fichier Init.js inclus dans la solution")

    *Fichier init.js inclus dans la solution*
8. Revenez à la **init.js** fichier à mettre à jour le **prêt** fonction de rappel.
9. À l’intérieur de la définition de rappel de fonction qui est passée à *prêt*, ajoutez le code suivant pour obtenir tous les éléments par un attribut de classe spécifique.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Appuyez sur **CTRL** + **espace** entre les guillemets à l’intérieur de la **getElementsByClassName** appel de fonction.

    ![Affichage d’IntelliSense pour la fonction getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "montrant l’IntelliSense pour la fonction getElementsByClassName")

    *Affichage d’IntelliSense pour la fonction getElementsByClassName*

    > [!NOTE]
    > Notez qu’IntelliSense affiche les classes définies dans les feuilles de style du projet.
11. Remplacez la ligne que vous avez créés avec le code suivant.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Placez le curseur après **Australie** les guillemets dans le **getElementsByTagName** (fonction), puis appuyez sur **CTRL** + **espace**.

    ![Affichage d’IntelliSense pour la méthode getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "montrant l’IntelliSense pour la méthode getElementByTagName")

    *Affichage d’IntelliSense pour la méthode getElementsByTagName*
13. Sélectionnez **&quot;audio&quot;** à partir de la liste et appuyez sur **entrée**. Le résultat est affiché dans l’illustration suivante.

    ![Récupération des éléments Audio](visual-studio-2013-web-tools/_static/image46.png "récupération des éléments Audio")

    *Récupération des éléments Audio*
14. Dans **l’Explorateur de solutions**, avec le bouton droit le **init.js** de fichiers dans le **Scripts** dossier et sélectionnez **ou les fichiers JavaScript de la réduction** à partir de la **Web Essentials** menu.

    ![Réduction ou les fichiers JavaScript](visual-studio-2013-web-tools/_static/image47.png "fichiers JavaScript de la réduction")

    *Réduction ou les fichiers JavaScript*
15. Lorsque vous êtes invité à activer la réduction automatique lorsque les modifications de fichier source, cliquez **Oui**.

    ![L’activation d’avertissement de réduction automatique](visual-studio-2013-web-tools/_static/image48.png "l’activation d’avertissement de réduction automatique")

    *L’activation de l’avertissement de réduction automatique*

    > [!NOTE]
    > Le **init.min.js** est créé et est ajouté en tant que dépendance de la **init.js** fichier.
    > 
    > ![Fichier init.min.js créé](visual-studio-2013-web-tools/_static/image49.png "fichier Init.min.js créé")
    > 
    > *Fichier init.min.js créé*
16. Ouvrez le **init.min.js** fichier et notez que le fichier est minimisé.

    ![Contenu du fichier init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js contenu du fichier")

    *Contenu du fichier init.min.js*
17. Dans le **init.js** , ajoutez le code suivant en dessous le **getElementsByTagName** appel pour lire tous les éléments audio de fonction.

    (Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Cliquez sur **CTRL** + **S** pour enregistrer le fichier. Dans la mesure où le fichier réduit est déjà ouvert, vous verrez une boîte de dialogue indiquant que le fichier a été modifié en dehors de l’éditeur de source. Cliquez sur **Oui**.

    ![Avertissement de Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "avertissement de Microsoft Visual Studio")

    *Avertissement de Microsoft Visual Studio*
19. Revenez à la **init.min.js** fichier pour vérifier que le fichier a été mis à jour avec le nouveau code.

    ![Mise à jour du fichier init.min.js](visual-studio-2013-web-tools/_static/image52.png "fichier Init.min.js mis à jour")

    *Fichier init.min.js mis à jour*
20. Cliquez sur le **Actualiser du navigateur lien** bouton.
21. Une fois que les deux navigateurs sont actualisées les lecteurs audio que vous avez vu dans la tâche précédente lecture commence automatiquement.

    ![Lecteur audio inclus dans la vue](visual-studio-2013-web-tools/_static/image53.png "lecteur Audio inclus dans la vue")

    *Lecteur audio inclus dans la vue*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En fin de cet atelier, vous avez appris comment :

- Utiliser les nouvelles fonctionnalités de l’éditeur HTML incluses dans Web Essentials, tels que des extraits de code HTML5 riches et Zen codage
- Utiliser les nouvelles fonctionnalités de l’éditeur CSS incluses dans Web Essentials telles que le sélecteur de couleurs et une info-bulle de la matrice navigateur
- Utiliser les nouvelles fonctionnalités de l’éditeur JavaScript incluses dans Web Essentials telles qu’extraction au fichier et IntelliSense pour tous les éléments HTML
- Échanger des données entre votre navigateur et de Visual Studio à l’aide du lien du navigateur
