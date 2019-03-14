---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Modification du code Web Forms ASP.NET dans Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 670f81ca1ef9923575cb2fee1747f06f426963d8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029706"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Édition du code Web Forms ASP.NET dans Visual Studio 2013
====================
par [Erik Reitan](https://github.com/Erikre)

Dans de nombreuses pages Web Form ASP.NET, écrivez du code dans Visual Basic, c# ou un autre langage. L’éditeur de code dans Visual Studio peut vous aider à écrire rapidement du code tout en vous aidant à éviter les erreurs. En outre, l’éditeur fournit des méthodes créer du code réutilisable pour aider à réduire la quantité de travail, que vous devez effectuer.

Cette procédure pas à pas illustre différentes fonctionnalités de l’éditeur de code Visual Studio.

Pendant cette procédure pas à pas, vous allez apprendre à :

- Corrigez les erreurs de code inline.
- Refactoriser et renommer le code.
- Renommer des variables et des objets.
- Insérer des extraits de code.

## <a name="prerequisites"></a>Prérequis


Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Le .NET Framework est installé automatiquement. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 et Microsoft Visual Studio Express 2013 pour le Web seront souvent être appelé Visual Studio tout au long de cette série de didacticiels.  
    >   
    > Si vous utilisez Visual Studio, cette procédure pas à pas suppose que vous avez choisi le **développement Web** collection de paramètres de la première fois que vous avez démarré Visual Studio. Pour plus d'informations, voir [Procédure : Sélectionnez les paramètres d’environnement de développement de Web](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Création d’un projet d’application Web et une Page

<a id="sectionToggle0"></a>

Dans cette partie de la procédure pas à pas, vous créez un projet d’application Web et ajoutez une nouvelle page à celui-ci.

### <a name="to-create-a-web-application-project"></a>Pour créer un projet d’application Web

1. Ouvrez Microsoft Visual Studio.
2. Dans le menu **Fichier**, sélectionnez **Nouveau projet**.  
    ![Menu fichier](code-editing-in-web-forms-pages/_static/image1.png)

    La boîte de dialogue **Nouveau projet** s’affiche.
3. Sélectionnez le **modèles**  - &gt; **Visual C#**  - &gt; **Web** groupe de modèles sur la gauche.
4. Choisissez le **Application Web ASP.NET** modèle dans la colonne centrale.
5. Nommez votre projet ***BasicWebApp*** et cliquez sur le **OK** bouton.   
![Boîte de dialogue Nouveau projet](code-editing-in-web-forms-pages/_static/image2.png)
6. Ensuite, sélectionnez le **Web Forms** modèle et cliquez sur le **OK** bouton pour créer le projet.  
![Boîte de dialogue Nouveau projet ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio crée un nouveau projet qui inclut les fonctionnalités prégénérées basée sur le modèle Web Forms.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Création d’une Page ASP.NET Web Forms


Lorsque vous créez une nouvelle application Web Forms à l’aide du **Application Web ASP.NET** modèle de projet, Visual Studio ajoute une page ASP.NET (page Web Forms) nommée *Default.aspx*, ainsi que plusieurs autres fichiers et les dossiers. Vous pouvez utiliser la *Default.aspx* page comme page d’accueil pour votre application Web. Toutefois, pour cette procédure pas à pas, vous créez et travailler avec une nouvelle page.

### <a name="to-add-a-page-to-the-web-application"></a>Pour ajouter une page à l’application Web


1. Dans **l’Explorateur de solutions**, cliquez sur le nom d’application Web (dans ce didacticiel est le nom de l’application **BasicWebSite**), puis cliquez sur **ajouter**  - &gt; **Un nouvel élément**.   
La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le **Visual C#**  - &gt; **Web** groupe de modèles sur la gauche. Ensuite, sélectionnez **Web Form** à partir du milieu de liste et nommez-le *PremièrePageWeb.aspx*.   
    ![Ajouter un nouvel élément](code-editing-in-web-forms-pages/_static/image4.png)
3. Cliquez sur **ajouter** pour ajouter la page Web Forms à votre projet.  
 Visual Studio crée la page et l’ouvre.
4. Ensuite, définissez cette nouvelle page comme page de démarrage par défaut. Dans **l’Explorateur de solutions**, avec le bouton droit de la nouvelle page nommée *PremièrePageWeb.aspx* et sélectionnez **définir comme Page de démarrage**. La prochaine fois que vous exécutez cette application pour tester notre progression, vous verrez automatiquement cette nouvelle page dans le navigateur.


## <a name="correcting-inline-coding-errors"></a>Correction des erreurs de codage de Inline


L’éditeur de code dans Visual Studio vous aide à éviter les erreurs que vous écrivez du code, et si vous avez apporté une erreur, l’éditeur de code vous aide à corriger l’erreur. Dans cette partie de la procédure pas à pas, vous allez écrire une ligne de code qui illustrent les fonctionnalités de correction d’erreur dans l’éditeur.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Pour corriger les erreurs de codage simples dans Visual Studio


1. Dans **conception** , double-cliquez sur la page vierge pour créer un gestionnaire pour le **charge** événement pour la page.   
   Vous utilisez le Gestionnaire d’événements uniquement comme un lieu d’écrire du code.
2. Dans le gestionnaire, tapez la ligne suivante qui contient une erreur et la presse **entrée**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Quand vous appuyez sur **entrée**, l’éditeur de code place soulignements verts et rouge (appellent généralement &quot;ondulée&quot; lignes) sous les zones du code qui ont des problèmes. Un soulignement vert indique un avertissement. Un trait de soulignement rouge indique une erreur indiquant que vous devez corriger. 

    Maintenez le pointeur de la souris sur `myStr` pour afficher une info-bulle qui vous indique sur l’avertissement. En outre, maintenez votre pointeur sur le trait de soulignement rouge pour voir le message d’erreur.

    L’illustration suivante montre le code avec des traits de soulignement.

    ![Texte de bienvenue en mode conception](code-editing-in-web-forms-pages/_static/image5.png "texte de bienvenue en mode Design")  
   L’erreur doit être résolu en ajoutant un point-virgule `;` à la fin de la ligne. L’avertissement vous informe simplement de que vous n’avez pas utilisé le `myStr` encore de variable.  

    > [!NOTE] 
    > 
    > Vous permet d’afficher votre code en cours mise en forme de paramètres dans Visual Studio en sélectionnant **outils**  - &gt; **Options**  - &gt; **polices et Couleurs**.


## <a name="refactoring-and-renaming"></a>Refactorisation et changement de nom

La refactorisation est une méthodologie logicielle qui implique la restructuration de votre code pour le rendre plus facile à comprendre et à gérer, tout en conservant ses fonctionnalités. Un exemple simple peut être qu’écrire du code dans un gestionnaire d’événements pour obtenir des données à partir d’une base de données. Lorsque vous développez votre page, vous découvrez que vous avez besoin accéder aux données à partir de plusieurs gestionnaires différents. Par conséquent, vous refactorisez du code de la page en créant une méthode d’accès aux données dans la page et en insérant des appels à la méthode dans les gestionnaires.

L’éditeur de code inclut des outils pour vous aider à effectuer diverses tâches de refactorisation. Dans cette procédure pas à pas, vous allez travailler avec deux techniques de refactorisation : renommer des variables et l’extraction des méthodes. Autres options de refactorisation incluent l’encapsulation des champs, la promotion des variables locales pour les paramètres de méthode et la gestion des paramètres de méthode. La disponibilité de ces options de refactorisation dépend de l’emplacement dans le code.

### <a name="refactoring-code"></a>Refactorisation du Code

Un scénario de refactorisation courant consiste à créer (extract) une méthode à partir du code qui se trouve dans un autre membre, tel qu’une méthode. Cela réduit la taille du membre d’origine et rend le code extrait réutilisable.

Dans cette partie de la procédure pas à pas, vous écrire un code simple et ensuite extraire une méthode à partir de celui-ci. Refactorisation est prise en charge pour c#, vous allez donc créer une page qui utilise c# comme langage de programmation.

### <a name="to-extract-a-method-in-a-c-page"></a>Pour extraire une méthode dans une page c#

1. Basculez vers **conception** vue.
2. Dans le **boîte à outils**, à partir de la **Standard** onglet, faites glisser un [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) contrôle sur la page.
3. Double-cliquez sur le **bouton** contrôle pour créer un gestionnaire pour son [cliquez sur](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) événement, puis ajoutez le code en surbrillance suivant :

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Le code crée un **ArrayList** objet, utilise une boucle pour le charger avec des valeurs, puis utilise une autre boucle pour afficher le contenu de la **ArrayList** objet.
4. Appuyez sur **CTRL + F5** à exécuter la page, puis cliquez sur le **bouton** pour vous assurer que vous voyez la sortie suivante :   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Revenir à l’éditeur de code, puis sélectionnez les lignes suivantes dans le Gestionnaire d’événements.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Avec le bouton droit de la sélection, cliquez sur **refactoriser**, puis choisissez **extraire la méthode**. 

    Le **extraire la méthode** boîte de dialogue s’affiche.
7. Dans le **nouveau nom de la méthode** , tapez **DisplayArray**, puis cliquez sur **OK**. 

    L’éditeur de code crée une nouvelle méthode nommée `DisplayArray`et place un appel à la nouvelle méthode dans le **cliquez sur** Gestionnaire où la boucle a été initialement.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Appuyez sur **CTRL + F5** à exécuter de nouveau la page, cliquez sur le **bouton**.

    La page fonctionne comme avant. Le `DisplayArray` méthode peut désormais être appel depuis n’importe où dans la classe de page.

## <a name="renaming-variables"></a>Renommer des Variables

Lorsque vous travaillez avec des variables, ainsi que des objets, vous souhaiterez peut-être les renommer une fois qu’ils sont déjà référencés dans votre code. Toutefois, le changement de nom des variables et objets peut provoquer le code arrêter l’exécution si vous omettez de renommer une des références. Par conséquent, vous pouvez utiliser la refactorisation pour effectuer le changement de nom.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Pour utiliser la refactorisation pour renommer une variable


1. Dans le **cliquez sur** Gestionnaire d’événements, recherchez la ligne suivante :

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Cliquez sur le nom de variable `alist`, choisissez **refactoriser**, puis choisissez **renommer**.

    Le **renommer** boîte de dialogue s’affiche.
3. Dans le **nouveau nom** , tapez **ArrayList1** et assurez-vous que le **aperçu des modifications de référence** case à cocher a été sélectionné. Cliquez ensuite sur **OK**.

    Le **aperçu des modifications** boîte de dialogue apparaît et affiche une arborescence qui contient toutes les références à la variable que vous renommez.
4. Cliquez sur **appliquer** pour fermer la **aperçu des modifications** boîte de dialogue.

    Les variables qui font référence en particulier à l’instance que vous avez sélectionnés sont renommés. Notez, cependant, que la variable `alist` dans la ligne suivante n’est pas renommé.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    La variable `alist` dans cette ligne n’est pas renommé, car il ne représente pas la même valeur que la variable `alist` que vous avez renommé. La variable `alist` dans le `DisplayArray` déclaration est une variable locale pour cette méthode. Cela illustre qu’à l’aide de la refactorisation pour renommer des variables est différente de celle simplement effectuer une action de rechercher et remplacer dans l’éditeur ; Renomme les variables avec une connaissance de la sémantique de la variable qui fonctionne avec la refactorisation.


## <a name="inserting-snippets"></a>Insertion d’extraits de code

Comme il existe de nombreuses tâches de codage que les développeurs Web Forms souvent amené à effectuer, l’éditeur de code fournit une bibliothèque d’extraits de code ou de blocs de code préécrit. Vous pouvez insérer ces extraits de code dans votre page.

Chaque langue que vous utilisez dans Visual Studio présente de légères différences dans la façon de qu'insérer des extraits de code. Pour plus d’informations sur l’insertion des extraits de code, consultez [extraits de Code IntelliSense Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx). Pour plus d’informations sur l’insertion des extraits de code dans Visual c#, consultez [extraits de Code Visual C#](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Étapes suivantes

Cette procédure pas à pas a illustré les fonctionnalités de base de l’éditeur de code Visual Studio 2010 pour corriger les erreurs dans votre code, la refactorisation de code, renommer des variables et insertion des extraits de code dans votre code. Fonctionnalités supplémentaires dans l’éditeur peuvent rendre le développement d’application rapide et facile. Vous pouvez, par exemple, décider d'effectuer les opérations suivantes :

- En savoir plus sur les fonctionnalités d’IntelliSense, telles que la modification des options IntelliSense, la gestion des extraits de code et recherche d’extraits de code en ligne. Pour plus d’informations, consultez [Utilisation d’IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Découvrez comment créer vos propres extraits de code. Pour plus d’informations, consultez [création et utilisation des extraits de Code IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- En savoir plus sur les fonctionnalités spécifiques à Visual Basic IntelliSense des extraits de code, comme la personnalisation des extraits de code et de résolution des problèmes. Pour plus d’informations, consultez [extraits de Code IntelliSense Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx)
- En savoir plus sur c#-fonctionnalités spécifiques d’IntelliSense, telles que des extraits de code et de refactorisation. Pour plus d’informations, consultez [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
