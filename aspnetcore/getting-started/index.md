---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Tutoriel : Bien démarrer avec ASP.NET Core

Ce tutoriel montre comment utiliser l’interface de ligne de commande .NET Core pour créer une application web ASP.NET Core.

Vous allez apprendre à :

> [!div class="checklist"]
> * Créer un projet application web.
> * Activer HTTPS localement.
> * Exécuter l’application.
> * Modifier une page Razor.

À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.

![Page d’accueil d’application web](_static/home-page.png)

## <a name="prerequisites"></a>Prérequis

* [SDK .NET Core 2.2](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>Créer un projet application web

Ouvrez un interpréteur de commandes, puis entrez la commande suivante :

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a>Activer HTTPS localement

Approuvez le certificat de développement HTTPS :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

La commande précédente affiche la boîte de dialogue suivante :

![Boîte de dialogue Avertissement de sécurité](~/getting-started/_static/cert.png)

Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

La commande précédente affiche le message suivant :

*L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’a pas encore été approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.
 
Cette commande peut vous demander votre mot de passe afin d’installer le certificat sur le trousseau système. Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS.

---

Pour plus d’informations, consultez [Approuver le certificat de développement ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Exécuter l'application

Exécutez les commandes suivantes :

```console
cd aspnetcoreapp
dotnet run
```

Une fois que l’interface de commande indique que l’application a démarré, accédez à [https://localhost:5001](https://localhost:5001). Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies. Cette application ne conserve pas les informations personnelles.

## <a name="edit-a-razor-page"></a>Modifier une page Razor

Ouvrez *Pages/Index.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Accédez à [https://localhost:5001](https://localhost:5001) et vérifiez que les changements sont affichés.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Créer un projet application web.
> * Activer HTTPS localement.
> * Exécuter le projet.
> * Effectuer une modification.

Pour en savoir plus sur ASP.NET Core, consultez le parcours d’apprentissage recommandé dans la présentation :

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
