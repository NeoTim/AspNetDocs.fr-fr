---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Appel des API Web à partir d’un Windows Phone 8 Application (C#)-ASP.NET 4.x
author: rmcmurray
description: 'Didacticiel avec le code : Créer une application API Web ASP.NET dans ASP.NET 4.x qui fournit un catalogue de livres à une application Windows Phone 8.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: a5c7804c2336e91dc171b5da52819436472e81cf
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59412448"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Appel de l’API web à partir d’une application Windows Phone 8 (C#)

par [Robert McMurray](https://github.com/rmcmurray)

Dans ce didacticiel, vous allez apprendre à créer un scénario complet de bout en bout consistant en une application API Web ASP.NET qui fournit un catalogue de livres à une application Windows Phone 8.

### <a name="overview"></a>Vue d'ensemble

Les services rESTful telles que l’ASP.NET Web API simplifient la création d’applications HTTP pour les développeurs en faisant abstraction de l’architecture pour les applications côté serveur et côté client. Au lieu de créer un protocole propriétaire basé sur un socket pour la communication, les développeurs Web API souhaitent publier les méthodes HTTP requis pour leur application, (par exemple : GET, POST, PUT, DELETE), et les développeurs d’applications client uniquement besoin de consommer les méthodes HTTP qui sont nécessaires à leur application.

Dans ce didacticiel de bout en bout, vous allez apprendre à utiliser des API Web pour créer les projets suivants :

- Dans le [première partie de ce didacticiel](#STEP1), vous allez créer une application API Web ASP.NET qui prend en charge toutes les opérations de création, lecture, mise à jour et suppression (CRUD) pour gérer un catalogue de livres. Cette application doit utiliser le [exemple de fichier XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) à partir de MSDN.
- Dans le [deuxième partie de ce didacticiel](#STEP2), vous allez créer une application Windows Phone 8 interactive qui Récupère les données à partir de votre application API Web.

#### <a name="prerequisites"></a>Prérequis

- Visual Studio 2013 avec le SDK Windows Phone 8 est installé
- Windows 8 ou version ultérieure sur un système 64 bits avec Hyper-V est installé
- Pour obtenir la liste d’exigences supplémentaires, consultez le *requise* section sur la [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) page de téléchargement.

> [!NOTE]
> Si vous vous apprêtez à tester la connectivité entre les API Web et les projets Windows Phone 8 sur votre système local, vous devez suivre les instructions fournies dans le *[connexion de l’émulateur de 8 Windows Phone pour les Applications d’API Web sur un ordinateur Local Ordinateur](https://go.microsoft.com/fwlink/?LinkId=324014)* article pour configurer votre environnement de test.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Étape 1 : Création du projet de librairie de l’API Web

La première étape de ce didacticiel de bout en bout consiste à créer un projet d’API Web qui prend en charge toutes les opérations CRUD ; Notez que vous allez ajouter le projet d’application Windows Phone à cette solution dans [étape 2](#STEP2) de ce didacticiel.

1. Ouvrez **Visual Studio 2013**.
2. Cliquez sur **fichier**, puis **nouveau**, puis **projet**.
3. Lorsque le **nouveau projet** boîte de dialogue, développez **installé**, puis **modèles**, puis **Visual C#**, puis **Web**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Cliquez sur l’image pour développer                                                                |


4. Mettez en surbrillance **Application Web ASP.NET**, entrez **BookStore** pour le nom du projet, puis cliquez sur **OK**.
5. Lorsque le **nouveau projet ASP.NET** boîte de dialogue s’affiche, sélectionnez le **API Web** modèle, puis cliquez sur **OK**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Cliquez sur l’image pour développer                                                                |


6. Lorsque le projet d’API Web s’ouvre, supprimer le contrôleur de l’exemple à partir du projet :

    1. Développez le **contrôleurs** dossier dans l’Explorateur de solutions.
    2. Cliquez sur le **ValuesController.cs** de fichiers, puis cliquez sur **supprimer**.
    3. Cliquez sur **OK** lorsque vous êtes invité à confirmer la suppression.
7. Ajouter un fichier de données XML au projet API Web ; Ce fichier contient le contenu du catalogue bookstore :

   1. Avec le bouton droit le **application\_données** dossier dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **un nouvel élément**.
   2. Lorsque le **ajouter un nouvel élément** boîte de dialogue s’affiche, mettez en surbrillance le **fichier XML** modèle.
   3. Nommez le fichier **Books.xml**, puis cliquez sur **ajouter**.
   4. Lorsque le **Books.xml** fichier est ouvert, remplacez le code dans le fichier avec le code XML à partir de l’exemple **books.xml** fichier sur MSDN : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Enregistrez et fermez le fichier XML.

8. Ajouter le modèle de librairie au projet API Web ; Ce modèle contient la logique de création, lecture, mise à jour et suppression (CRUD) pour l’application bookstore :

   1. Cliquez sur le **modèles** dossier dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **classe**.
   2. Lorsque le **ajouter un nouvel élément** boîte de dialogue s’affiche, nommez le fichier de classe **BookDetails.cs**, puis cliquez sur **ajouter**.
   3. Lorsque le **BookDetails.cs** fichier est ouvert, remplacez le code dans le fichier par le code suivant : 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Enregistrez et fermez le **BookDetails.cs** fichier.

9. Ajouter le contrôleur de librairie au projet API Web :

   1. Avec le bouton droit le **contrôleurs** dossier dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **contrôleur**.
   2. Lorsque le **ajouter une structure** boîte de dialogue s’affiche, mettez en surbrillance **contrôleur Web API 2 - vide**, puis cliquez sur **ajouter**.
   3. Lorsque le **ajouter un contrôleur** boîte de dialogue s’affiche, nommez le contrôleur **BooksController**, puis cliquez sur **ajouter**.
   4. Lorsque le **BooksController.cs** fichier est ouvert, remplacez le code dans le fichier par le code suivant : 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Enregistrez et fermez le **BooksController.cs** fichier.

10. Générez l’application API Web pour rechercher les erreurs.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Étape 2 : Ajouter le projet Windows Phone 8 Bookstore Catalog

L’étape suivante de ce scénario de bout en bout consiste à créer l’application de catalogue pour Windows Phone 8. Cette application doit utiliser le *Windows Phone Databound application* modèle pour l’interface utilisateur par défaut et qu’il utilisera l’application API Web que vous avez créé dans [étape 1](#STEP1) de ce didacticiel en tant que la source de données.

1. Avec le bouton droit le **BookStore** solution dans le dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis **nouveau projet**.
2. Lorsque le **nouveau projet** boîte de dialogue, développez **installé**, puis **Visual C#**, puis **Windows Phone**.
3. Mettez en surbrillance **Windows Phone Databound application**, entrez **BookCatalog** pour le nom, puis cliquez sur **OK**.
4. Ajoutez le package NuGet de Json.NET pour la **BookCatalog** projet :

    1. Avec le bouton droit **références** pour le **BookCatalog** de projet dans l’Explorateur de solutions, puis cliquez sur **gérer les Packages NuGet**.
    2. Lorsque le **gérer les Packages NuGet** boîte de dialogue s’affiche, développez le **Online** section et mettez en surbrillance **nuget.org**.
    3. Entrez **Json.NET** dans la recherche de champ et cliquez sur l’icône de recherche.
    4. Mettez en surbrillance **Json.NET** dans les résultats de recherche, puis cliquez sur **installer**.
    5. Une fois l’installation terminée, cliquez sur **fermer**.
5. Ajouter le **BookDetails** modèle pour le **BookCatalog** projet ; il contient un modèle générique de la classe bookstore :

   1. Cliquez sur le **BookCatalog** de projet dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **nouveau dossier**.
   2. Nommez le nouveau dossier **modèles**.
   3. Cliquez sur le **modèles** dossier dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **classe**.
   4. Lorsque le **ajouter un nouvel élément** boîte de dialogue s’affiche, nommez le fichier de classe **BookDetails.cs**, puis cliquez sur **ajouter**.
   5. Lorsque le **BookDetails.cs** fichier est ouvert, remplacez le code dans le fichier par le code suivant : 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Enregistrez et fermez le **BookDetails.cs** fichier.

6. Mise à jour le **MainViewModel.cs** classe pour inclure les fonctionnalités pour communiquer avec l’application API Web de librairie :

   1. Développez le **ViewModels** dossier dans l’Explorateur de solutions, puis double-cliquez sur le **MainViewModel.cs** fichier.
   2. Lorsque le **MainViewModel.cs** fichier est ouvert, remplacez le code dans le fichier par le code suivant ; Notez que vous devez mettre à jour la valeur de la `apiUrl` constante avec l’URL réelle de votre API Web : 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Enregistrez et fermez le **MainViewModel.cs** fichier.

7. Mise à jour le **MainPage.xaml** fichier pour personnaliser le nom de l’application :

   1. Double-cliquez sur le **MainPage.xaml** fichier dans l’Explorateur de solutions.
   2. Lorsque le **MainPage.xaml** fichier est ouvert, recherchez les lignes de code suivantes : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Remplacez ces lignes avec les éléments suivants : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Enregistrez et fermez le **MainPage.xaml** fichier.

8. Mise à jour le **DetailsPage.xaml** fichier pour personnaliser les éléments affichés :

   1. Double-cliquez sur le **DetailsPage.xaml** fichier dans l’Explorateur de solutions.
   2. Lorsque le **DetailsPage.xaml** fichier est ouvert, recherchez les lignes de code suivantes : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Remplacez ces lignes avec les éléments suivants : 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Enregistrez et fermez le **DetailsPage.xaml** fichier.

9. Générez l’application Windows Phone pour rechercher les erreurs.

### <a name="step-3-testing-the-end-to-end-solution"></a>Étape 3 : Test de la Solution de bout en bout

Comme mentionné dans le *prérequis* section de ce didacticiel, lorsque vous testez la connectivité entre les API Web et Windows Phone 8 projets sur votre système local, vous devez suivre les instructions fournies dans le *[ Connexion de l’émulateur de 8 Windows Phone pour les Applications d’API Web sur un ordinateur Local](https://go.microsoft.com/fwlink/?LinkId=324014)* article pour configurer votre environnement de test.

Une fois que vous avez configuré l’environnement de test, vous devez définir l’application Windows Phone comme projet de démarrage. Pour ce faire, mettez en surbrillance le **BookCatalog** application dans l’Explorateur de solutions, puis cliquez sur **définir comme projet de démarrage**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Cliquez sur l’image pour développer |

Lorsque vous appuyez sur F5, Visual Studio démarre à la fois le Windows Phone émulateur, qui affiche un &quot;Patientez&quot; message alors que les données d’application sont récupérées à partir de votre API Web :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Cliquez sur l’image pour développer |

Si tout est réussie, vous devez voir le catalogue affiché :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Cliquez sur l’image pour développer |

Si vous appuyez sur n’importe quel titre de livre, l’application affiche la description du livre :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Cliquez sur l’image pour développer |

Si l’application ne peut pas communiquer avec votre API Web, un message d’erreur s’affichera :

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Cliquez sur l’image pour développer |

Si vous appuyez sur le message d’erreur, des détails supplémentaires sur l’erreur seront affiche :


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Cliquez sur l’image pour développer                                                                 |

