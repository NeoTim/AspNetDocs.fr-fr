---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Création d’une application MVC 3 avec Razor et JavaScript discret (fr) Microsoft Docs
author: rick-anderson
description: L’exemple d’application Web de la liste d’utilisateurs démontre à quel point il est simple de créer ASP.NET applications MVC 3 à l’aide du moteur Razor View. L’exemple d’application s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: c57e19f8eeca15e3676b3d490b08f69786d44f93
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542454"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Création d’une application MVC 3 Application avec Razor et JavaScript non obstrusif

par [Microsoft](https://github.com/microsoft)

> L’exemple d’application Web de la liste d’utilisateurs démontre à quel point il est simple de créer ASP.NET applications MVC 3 à l’aide du moteur Razor View. L’application d’échantillon montre comment utiliser le nouveau moteur razor view avec ASP.NET version 3 et Visual Studio 2010 pour créer un site Web fictif de la liste des utilisateurs qui comprend des fonctionnalités telles que la création, l’affichage, l’édition et la suppression des utilisateurs.
> 
> Ce tutoriel décrit les mesures qui ont été prises afin de construire l’échantillon de la liste d’utilisateurs ASP.NET application MVC 3. Un projet Visual Studio avec le code source C et VB est disponible pour accompagner ce sujet : [Télécharger](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Si vous avez des questions sur ce tutoriel, s’il vous plaît les poster sur le [forum MVC](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Vue d’ensemble

L’application que vous allez construire est un site Web simple liste d’utilisateurs. Les utilisateurs peuvent entrer, afficher et mettre à jour les informations utilisateur.

![Site d’échantillon](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Vous pouvez télécharger le projet VB et C ' terminé [ici](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Création de l’application Web

Pour commencer le tutoriel, ouvrez Visual Studio 2010 et créez un nouveau projet à l’aide du modèle *d’application Web MVC 3 ASP.NET.* Nommez &quot;l’application Mvc3Razor&quot;.

[![Nouveau projet MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Dans le **new ASP.NET MVC 3 Project** dialogue, sélectionnez application **Internet**, sélectionnez le moteur Razor view, puis cliquez **sur OK**.

![Nouveau ASP.NET dialogue du projet MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

Dans ce tutoriel, vous n’utiliserez pas le fournisseur d’adhésion ASP.NET, de sorte que vous pouvez supprimer tous les fichiers associés au logon et à l’adhésion. Dans **Solution Explorer**, supprimez les fichiers et répertoires suivants :

- *Contrôleurs-AccountController*
- *Modèles-AccountModels*
- *Vues-_LogOnPartial\\partagées*
- *Vues-Compte* (et tous les fichiers dans cet annuaire)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Modifier `<div>` le `logindisplay` <em>&quot;</em>&quot; <em> \_fichier Layout.cshtml</em> et remplacer la balisage à l’intérieur de l’élément nommé par le message Login Disabled . L’exemple suivant montre la nouvelle balisage :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Ajout du modèle

Dans **Solution Explorer**, cliquez à droite sur le dossier *Modèles,* sélectionnez **Ajouter,** puis cliquez sur **Classe**.

![Nouvelle classe d’utilisateur Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nommez la classe `UserModel`. Remplacer le contenu du fichier *UserModel* par le code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

La `UserModel` classe représente les utilisateurs. Chaque membre de la classe est annoté avec [l’attribut requis](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) à partir de l’espace de nom [DataAnnotations.](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Les attributs de l’espace de nom [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fournissent une validation automatique côté client et serveur pour les applications Web.

Ouvrez `HomeController` la classe `using` et ajoutez une `UserModel` directive `Users` afin que vous puissiez accéder à la et aux classes :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Juste après `HomeController` la déclaration, ajoutez le commentaire `Users` suivant et la référence à une classe :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

La `Users` classe est un magasin de données simplifié et en mémoire que vous utiliserez dans ce tutoriel. Dans une application réelle, vous utiliseriez une base de données pour stocker les informations utilisateur. Les premières lignes `HomeController` du fichier sont affichées dans l’exemple suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Construisez l’application de sorte que le modèle d’utilisateur sera disponible pour l’assistant d’échafaudage dans la prochaine étape.

## <a name="creating-the-default-view"></a>Création de la vue par défaut

L’étape suivante consiste à ajouter une méthode d’action et de visualiser pour afficher les utilisateurs.

Supprimer le fichier *d’index Views-Home.* Vous créerez un nouveau fichier *Index* pour afficher les utilisateurs.

Dans `HomeController` la classe, remplacez `Index` le contenu de la méthode par le code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Cliquez à droite `Index` à l’intérieur de la méthode, puis cliquez sur **Ajouter Voir**.

![Ajouter la vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Sélectionnez **l’option Créer une vue fortement typée.** Pour **la classe de données View**, sélectionnez **Mvc3Razor.Models.UserModel**. (Si vous ne voyez pas **Mvc3Razor.Models.UserModel** dans la boîte **de classe de données View,** vous devez construire le projet.) Assurez-vous que le moteur de vue est réglé sur **Razor**. Définir **le contenu** de la **liste,** puis cliquez sur **Ajouter**.

![Ajouter la vue de l’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nouvelle vue échafaude automatiquement les données `Index` utilisateur qui sont transmises à la vue. Examinez le fichier *Views-Home Index* nouvellement généré. Les **liens Créer de nouveaux,** **modifier,** **détails**et **supprimer** ne fonctionnent pas, mais le reste de la page est fonctionnel. Exécutez la page. Vous voyez une liste d’utilisateurs.

![Page d’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Ouvrez le fichier *Index.cshtml* et remplacez `ActionLink` la balisage pour **Modifier**, **Détails**, et **Supprimer** avec le code suivant:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Le nom d’utilisateur est utilisé comme ID pour trouver l’enregistrement sélectionné dans les liens **Modifier**, **Détails**et **Supprimer.**

## <a name="creating-the-details-view"></a>Création de la vue des détails

L’étape suivante consiste `Details` à ajouter une méthode d’action et une vue afin d’afficher les détails de l’utilisateur.

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Ajoutez la `Details` méthode suivante au contrôleur à domicile :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Cliquez à droite `Details` à l’intérieur de la méthode, puis sélectionnez <strong>Add View</strong>. Vérifiez que la boîte <strong>de classe de données View</strong> contient <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Définissez le contenu de <strong>l’affiche</strong> <strong>sur les détails,</strong> puis cliquez sur <strong>Ajouter</strong>.

![Ajouter la vue des détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Exécutez l’application et sélectionnez un lien de détails. L’échafaudage automatique montre chaque propriété du modèle.

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Création de la vue d’édition

Ajoutez la `Edit` méthode suivante au contrôleur à domicile.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Ajoutez une vue comme dans les étapes précédentes, mais définissez **le contenu De vue** pour **modifier**.

![Ajouter la vue Edit](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Exécutez l’application et modifiez le prénom et le nom de famille de l’un des utilisateurs. Si vous `DataAnnotation` violez les contraintes `UserModel` qui ont été appliquées à la classe, lorsque vous soumettez le formulaire, vous verrez des erreurs de validation qui sont produites par le code serveur. Par exemple, si vous &quot;changez le prénom Ann&quot; en &quot;A&quot;, lorsque vous soumettez le formulaire, l’erreur suivante s’affiche sur le formulaire :

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

Dans ce tutoriel, vous traitez le nom d’utilisateur comme la clé principale. Par conséquent, la propriété de nom d’utilisateur ne peut pas être changée. Dans le fichier *Edit.cshtml,* juste après la `Html.BeginForm` déclaration, définissez le nom d’utilisateur comme un champ caché. Cela provoque la propriété à passer dans le modèle. Le fragment de code suivant `Hidden` montre le placement de la déclaration :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Remplacez `TextBoxFor` `ValidationMessageFor` le nom d’utilisateur `DisplayFor` et le balisage par un appel. La `DisplayFor` méthode affiche la propriété comme un élément de lecture seulement. L'exemple suivant montre le balisage complet. `TextBoxFor` L’original `ValidationMessageFor` et les appels sont commentés avec le`@* *@`Razor commence-commentaire et les personnages de fin de commentaire ( )

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Permettre la validation côté client

Pour activer la validation côté client dans ASP.NET MVC 3, vous devez définir deux drapeaux et inclure trois fichiers JavaScript.

Ouvrez le fichier *Web.config* de l’application. Vérifier `that ClientValidationEnabled` `UnobtrusiveJavaScriptEnabled` et sont configurés dans les paramètres de l’application. Le fragment suivant à partir du fichier *Web.config* racine affiche les paramètres corrects :

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Le `UnobtrusiveJavaScriptEnabled` réglage à vrai permet l’Ajax discret et la validation discrète du client. Lorsque vous utilisez une validation discrète, les règles de validation sont transformées en attributs HTML5. Les noms d’attributs HTML5 ne peuvent se composer que de lettres, de chiffres et de tirets minuscules.

Le `ClientValidationEnabled` réglage à vrai permet la validation côté client. En définissant ces clés dans le fichier *Web.config* d’application, vous activez la validation du client et JavaScript discret pour l’ensemble de l’application. Vous pouvez également activer ou désactiver ces paramètres dans des vues individuelles ou dans les méthodes de contrôleur à l’aide du code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Vous devez également inclure plusieurs fichiers JavaScript dans la vue rendue. Un moyen facile d’inclure le JavaScript dans toutes les vues est de les ajouter au fichier *Views-Shared\\_Layout.cshtml.* Remplacer `<head>` l’élément du fichier * \_Layout.cshtml* par le code suivant :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Les deux premiers scripts jQuery sont hébergés par le Microsoft Ajax Content Delivery Network (CDN). En profitant du CDN Microsoft Ajax, vous pouvez améliorer considérablement les performances de vos applications.

Exécutez l’application et cliquez sur un lien de modification. Afficher la source de la page dans le navigateur. La source du navigateur affiche `data-val` de nombreux attributs du formulaire (pour la validation des données). Lorsque la validation du client et JavaScript discret sont activés, les `data-val="true"` champs d’entrée avec une règle de validation client contiennent l’attribut pour déclencher une validation discrète du client. Par exemple, `City` le champ du modèle a été décoré avec [l’attribut requis,](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) ce qui donne le HTML indiqué dans l’exemple suivant :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Pour chaque règle de validation du client, `data-val-rulename="message"`un attribut est ajouté qui a le formulaire . À `City` l’aide de l’exemple de terrain `data-val-required` montré plus &quot;tôt, la&quot;règle de validation requise du client génère l’attribut et le message Le champ de la Ville est nécessaire. Exécutez l’application, modifiez l’un des utilisateurs et dégagez le `City` champ. Lorsque vous sortez du champ, vous voyez un message d’erreur de validation côté client.

![Ville requise](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

De même, pour chaque paramètre de la règle de `data-val-rulename-paramname=paramvalue`validation du client, un attribut est ajouté qui a le formulaire . Par exemple, `FirstName` la propriété est annotée avec l’attribut [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) et spécifie une longueur minimale de 3 et une longueur maximale de 8. La règle de `length` validation des `max` données nommée a le nom du paramètre et la valeur du paramètre 8. Ce qui suit affiche le `FirstName` HTML qui est généré pour le champ lorsque vous modifiez l’un des utilisateurs:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Pour plus d’informations sur la validation discrète du client, voir l’entrée [Nonobtrusive Client Validation dans ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) dans le blog de Brad Wilson.

> [!NOTE]
> Dans ASP.NET MVC 3 Beta, vous devez parfois soumettre le formulaire afin de commencer la validation côté client. Cela pourrait être modifié pour la version finale.

## <a name="creating-the-create-view"></a>Création de la vue de création

L’étape suivante consiste `Create` à ajouter une méthode d’action et une vue afin de permettre à l’utilisateur de créer un nouvel utilisateur. Ajoutez la `Create` méthode suivante au contrôleur à domicile :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Ajoutez une vue comme dans les étapes précédentes, mais définissez **le contenu De vue** pour **créer**.

![Créer une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Exécutez l’application, sélectionnez le lien **Créer** et ajoutez un nouvel utilisateur. La `Create` méthode tire automatiquement parti de la validation côté client et côté serveur. Essayez d’entrer un nom d’utilisateur &quot;qui&quot;contient de l’espace blanc, comme Ben X . Lorsque vous sortez du champ de nom d’utilisateur, une erreur de validation côté client (`White space is not allowed`) s’affiche.

## <a name="add-the-delete-method"></a>Ajouter la méthode Supprimer

Pour compléter le tutoriel, `Delete` ajoutez la méthode suivante au contrôleur à domicile :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Ajoutez `Delete` une vue comme dans les étapes précédentes, en définissant **le contenu Afficher** à **supprimer**.

![Supprimer la vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Vous disposez désormais d’une application MVC 3 simple mais entièrement ASP.NET fonctionnelle avec validation.
