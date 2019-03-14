---
title: Gérer les dépendances côté client avec Bower dans ASP.NET Core
author: rick-anderson
description: La gestion des packages côté client avec Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 06edf7ee791aac0984ff71c2f243f61093f0d503
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047986"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Gérer les dépendances côté client avec Bower dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel riz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), et [Scott Addie](https://scottaddie.com)

> [!IMPORTANT]
> Tandis que Bower est conservé, son chargés de maintenance vous recommandons d’utiliser une autre solution. [Gestionnaire de bibliothèque](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan en abrégé) est outil de Visual Studio nouvelle bibliothèque côté client acquisition (Visual Studio 15,8 ou version ultérieure). Pour plus d'informations, consultez <xref:client-side/libman/index>. Bower est pris en charge dans Visual Studio jusqu'à la version 15.5.
>
>  L'utilisation de Yarn avec Webpack constitue une alternative courante pour laquelle des [instructions de migration](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) sont disponibles.

[Bower](https://bower.io/) se définit lui-même comme "Un gestionnaire de paquets pour le web". Dans l’écosystème .NET, il remplit le vide laissé par l’impossibilité d'inclure des fichiers de contenu statique avec NuGet. Pour les projets ASP.NET Core, ces fichiers statiques sont inhérents aux bibliothèques côté client comme[jQuery](http://jquery.com/) et [Bootstrap](http://getbootstrap.com/). Pour les bibliothèques .NET, vous utilisez toujours le gestionnaire de package [NuGet](https://www.nuget.org/)

Les nouveaux projets créés avec les modèles de projet ASP.NET Core sont configurés avec la génération côté client. [jQuery](http://jquery.com/) et [Bootstrap](http://getbootstrap.com/) sont installés, et Bower est pris en charge.

Les packages côté client sont répertoriés dans le fichier *bower.json*. Les modèles de projet ASP.NET Core configurent *bower.json* avec jQuery, la validation jQuery et Bootstrap.

Dans ce didacticiel, nous allons ajouter la prise en charge de [Font Awesome](http://fontawesome.io). Les packages Bower peuvent être installés avec l'interface graphique **Gérer les packages Bower** ou manuellement dans le fichier *bower.json*.

### <a name="installation-via-manage-bower-packages-ui"></a>Installation par le biais de gérer les Packages Bower l’interface utilisateur

* Créez une application web ASP.NET Core avec le modèle **Application web ASP.NET Core (.NET Core)**. Sélectionnez **Application Web** et **Aucune authentification**.

* Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions et sélectionnez **Gérer les packages Bower** (ou dans le menu principal, **Projet** > **Gérer les packages Bower**).

* Dans la fenêtre **Bower : \<nom du projet\>** cliquez sur l’onglet "Parcourir", puis filtrer la liste des packages en entrant `font-awesome` dans la zone de recherche :

  ![gérer les packages bower](bower/_static/manage-bower-packages.png)

* Vérifiez que le « enregistrer les modifications de *bower.json*« case à cocher est activée. Sélectionnez une version dans la liste déroulante et cliquez sur le bouton **Installer**. Le fenêtre de **sortie** affiche les détails d’installation.

### <a name="manual-installation-in-bowerjson"></a>Installation manuelle de bower.json

Ouvrez le fichier *bower.json* et ajoutez "Font Awesome" aux dépendances.  IntelliSense affiche les packages disponibles.  Lorsqu’un package est sélectionné, les versions disponibles sont affichées.   Les images ci-dessous sont plus anciennes et ne correspondent pas à ce que vous voyez.

![IntelliSense de l’Explorateur de package bower](bower/_static/add-package.png)

![version de bower IntelliSense](bower/_static/version-intelliSense.png)

Bower utilise le [contrôle de version sémantique](http://semver.org/) pour organiser les dépendances.  les dépendances.  Le contrôle de version sémantique, également appelé SemVer, identifie les packages avec le modèle de numérotation \<majeure >.\< mineure >. \<correctif >. IntelliSense simplifie le contrôle de version sémantique en affichant uniquement quelques choix courants. Le premier élément dans la liste IntelliSense (4.6.3 dans l’exemple ci-dessus) est considéré comme la dernière version stable du package. Le symbole de signe insertion (^) correspond à la version majeure la plus récente et le tilde (~) correspond à la version mineure la plus récente.

Enregistrez le fichier *bower.json*. Visual Studio surveille les modifications du fichier *bower.json*. Après l’enregistrement, la commande *bower install* est exécutée. Consultez la fenêtre de sortie et la vue**Bower/npm** pour la commande exacte exécutée.

Ouvrez le fichier *.bowerrc*sous*bower.json*. La propriété `directory` est configurée sur *wwwroot/lib*  qui indique l’emplacement où Bower installera les composants de package.

```json
{
 "directory": "wwwroot/lib"
}
```

Vous pouvez utiliser la zone de recherche dans l’Explorateur de solutions pour rechercher et afficher le package font-awesome.

Ouvrez le fichier *Views\Shared\_Layout.cshtml* et ajoutez le fichier CSS font-awesome au[TagHelper](xref:mvc/views/tag-helpers/intro) de l’environnement pour `Development`. À partir de l’Explorateur de solutions, faites glisser et déposez*font-awesome.css* à l’intérieur de l’élément `<environment names="Development">`.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

Dans une application de production, vous ajouteriez *font-awesome.min.css* au tag helper de l'environnement pour `Staging,Production`.

Remplacez le contenu du fichier Razor *Views\Home\About.cshtml* par le balisage suivant :

[!code-html[](bower/sample/About.cshtml)]

Exécutez l’application et accédez à la vue About pour vérifier le fonctionnement du package font-awesome.

## <a name="exploring-the-client-side-build-process"></a>Explorer le processus de génération du côté client

La plupart des modèles de projet ASP.NET Core sont déjà configurés pour utiliser Bower. Cette procédure pas à pas commence par un projet ASP.NET Core vide et ajoute chaque élément manuellement pour vous donner une idée de l’utilisation de Bower dans un projet. Vous pouvez voir ainsi ce qui arrive à la structure du projet et le résultat de l’exécution chaque fois que la configuration est modifiée.

Les étapes globales à utiliser pour le processus de génération côté client avec Bower sont :

* Définir les paquets utilisés dans votre projet. <!-- once defined, you don't need to download them, VS does -->
* Référencer des paquets à partir de vos pages web.

### <a name="define-packages"></a>Définir des packages

Une fois la liste de paquets définie dans le fichier *bower.json*, Visual Studio télécharge les paquets. L’exemple suivant utilise Bower pour charger jQuery et Bootstrap dans le dossier *wwwroot*.

* Créez une application web ASP.NET Core avec le modèle **Application web ASP.NET Core (.NET Core)**. Sélectionnez le modèle de projet **vide** et cliquez sur **OK**.

* Dans l’Explorateur de solutions, cliquez sur le projet > **Ajouter un nouvel élément** et sélectionnez **Fichier de configuration Bower**. Remarque : Un *.bowerrc* fichier est également ajouté.

* Ouvrez le fichier *bower.json*et ajoutez jquery et bootstrap dans la section `dependencies`. Le fichier *bower.json* résultant ressemblera à l’exemple suivant. Les versions changent au fil du temps et peuvent ne pas correspondre à l’image ci-dessous.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Enregistrez le fichier *bower.json*.

  Vérifiez que le projet inclut les répertoires *Bootstrap* et *jQuery* dans *wwwroot/lib*. Bower utilise le fichier *.bowerrc* pour installer les composants dans *wwwroot/lib*.

  Remarque : L’interface utilisateur « Gérer les Packages Bower » fournit une alternative à la modification de fichier manuel.

### <a name="enable-static-files"></a>Activer les fichiers statiques

* Ajoutez le paquet NuGet `Microsoft.AspNetCore.StaticFiles` au projet.
* Activez le support des fichiers statiques grâce à [l’intergiciel (middleware) de service des fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions). Ajoutez un appel à [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) à la méthode `Configure` de la classe `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Référence à des packages

Dans cette section, vous allez créer une page HTML pour vérifier qu’il peut accéder aux packages déployés.

* Ajoutez une nouvelle page HTML nommée *Index.html* dans le dossier *wwwroot*. Remarque : Vous devez ajouter le fichier HTML pour le *wwwroot* dossier. Remarque : Vous devez ajouter le fichier HTML dans le dossier *wwwroot*. Consultez [fichiers statiques](xref:fundamentals/static-files) pour plus d’informations.

  Remplacez le contenu du fichier *Index.html* par le contenu suivant :

[!code-html[](bower/sample/Index.html)]

* Exécutez l’application et accédez à `http://localhost:<port>/Index.html`. Vous pouvez également, à partir du fichier *Index.html* ouvert, appuyer sur `Ctrl+Shift+W`. Vérifiez que le style jumbotron est appliqué, que le code jQuery répond quand le bouton fait l'objet d'un clic et que le bouton Bootstrap change d’état.

  ![style JumboTron appliqué](bower/_static/jumbotron.png)
