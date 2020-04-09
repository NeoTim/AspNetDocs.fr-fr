---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Caisse et paiement avec PayPal (fr) Microsoft Docs
author: Erikre
description: Cette série de tutoriels vous enseignera les bases de la construction d’une application ASP.NET Web Forms en utilisant ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour We...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676054"
---
# <a name="checkout-and-payment-with-paypal"></a>Commande et paiement avec PayPal

par [Erik Reitan](https://github.com/Erikre)

[Téléchargez Wingtip Toys Sample Project (C)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Téléchargez le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de tutoriels vous enseignera les bases de la construction d’une application ASP.NET Web Forms en utilisant ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web. Un projet Visual Studio 2013 [avec le code source C est](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) disponible pour accompagner cette série de tutoriels.

Ce tutoriel décrit comment modifier l’application d’échantillon Wingtip Toys pour inclure l’autorisation de l’utilisateur, l’enregistrement et le paiement à l’aide de PayPal. Seuls les utilisateurs connectés auront l’autorisation d’acheter des produits. La fonctionnalité d’enregistrement intégrée du modèle de projet ASP.NET 4.5 Web Forms comprend déjà une grande partie de ce dont vous avez besoin. Vous ajouterez la fonctionnalité PayPal Express Checkout. Dans ce tutoriel, vous utilisez l’environnement de test de développeur PayPal, de sorte qu’aucun fonds réel ne sera transféré. À la fin du tutoriel, vous testerez l’application en sélectionnant des produits à ajouter au panier d’achat, en cliquant sur le bouton de caisse et en transférant des données sur le site Web de test PayPal. Sur le site De test PayPal, vous confirmerez vos informations d’expédition et de paiement, puis retournerez à l’application locale Wingtip Toys pour confirmer et compléter l’achat.

Il existe plusieurs processeurs de paiement tiers expérimentés qui se spécialisent dans les achats en ligne qui répondent à l’évolutivité et la sécurité. ASP.NET les développeurs devraient tenir compte des avantages d’utiliser une solution de paiement tierce avant de mettre en œuvre une solution d’achat et d’achat.

> [!NOTE] 
> 
> L’application d’échantillon Wingtip Toys a été conçue pour montrer des concepts et des fonctionnalités spécifiques de ASP.NET disponibles pour ASP.NET les développeurs Web. Cette application d’échantillon n’a pas été optimisée pour toutes les circonstances possibles en ce qui concerne l’évolutivité et la sécurité.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment restreindre l’accès à des pages spécifiques dans un dossier.
- Comment créer un caddie connu à partir d’un caddie anonyme.
- Comment activer SSL pour le projet.
- Comment ajouter un fournisseur OAuth au projet.
- Comment utiliser PayPal pour acheter des produits en utilisant l’environnement de test PayPal.
- Comment afficher les détails de PayPal dans un contrôle **DetailsView.**
- Comment mettre à jour la base de données de l’application Wingtip Toys avec les détails obtenus auprès de PayPal.

## <a name="adding-order-tracking"></a>Ajout de suivi des commandes

Dans ce tutoriel, vous allez créer deux nouvelles classes pour suivre les données de l’ordre qu’un utilisateur a créé. Les classes feront le suivi des données concernant les informations d’expédition, le total des achats et la confirmation des paiements.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Ajouter les classes de modèle Order and OrderDetail

Plus tôt dans cette série de tutoriels, vous avez défini le `Category`schéma `Product`pour `CartItem` les catégories, les produits et les articles de panier d’achat en créant le , , et les classes dans le dossier *Modèles.* Maintenant, vous allez ajouter deux nouvelles classes pour définir le schéma pour la commande de produit et les détails de l’ordre.

1. Dans le dossier **Models,** ajoutez une nouvelle classe nommée *Order.cs*.   
   Le nouveau fichier de classe est affiché dans l’éditeur.
2. Remplacez le code par défaut par ce qui suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Ajoutez une *classe OrderDetail.cs* au dossier *Models.*
4. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Les `Order` `OrderDetail` classes et les classes contiennent le schéma pour définir les informations de commande utilisées pour l’achat et l’expédition.

En outre, vous devrez mettre à jour la classe de contexte de base de données qui gère les classes d’entités et qui fournit un accès aux données à la base de données. Pour ce faire, vous ajouterez `OrderDetail` les classes `ProductContext` nouvellement créées de l’Ordre et des modèles à la classe.

1. Dans **Solution Explorer**, trouvez et ouvrez le fichier *ProductContext.cs.*
2. Ajoutez le code mis en évidence au fichier *ProductContext.cs* tel qu’indiqué ci-dessous :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Comme mentionné précédemment dans cette série *ProductContext.cs* de tutoriels, `System.Data.Entity` le code dans le fichier ProductContext.cs ajoute l’espace de nom de sorte que vous avez accès à toutes les fonctionnalités de base du cadre d’entité. Cette fonctionnalité inclut la capacité d’interroger, d’insérer, de mettre à jour et de supprimer des données en travaillant avec des objets fortement tapés. Le code ci-dessus de la `ProductContext` classe `Order` ajoute `OrderDetail` l’accès au Cadre d’entités aux classes nouvellement ajoutées.

## <a name="adding-checkout-access"></a>Ajout de l’accès à la caisse

L’application d’échantillon Wingtip Toys permet aux utilisateurs anonymes d’examiner et d’ajouter des produits à un panier d’achat. Toutefois, lorsque les utilisateurs anonymes choisissent d’acheter les produits qu’ils ont ajoutés au panier, ils doivent se connecter au site. Une fois qu’ils se sont connectés, ils peuvent accéder aux pages restreintes de l’application Web qui gèrent le processus de caisse et d’achat. Ces pages restreintes sont contenues dans le dossier *Checkout* de l’application.

### <a name="add-a-checkout-folder-and-pages"></a>Ajouter un dossier et des pages de caisse

Vous allez maintenant créer le dossier *Checkout* et les pages qu’il y verra pendant le processus de caisse. Vous mettre à jour ces pages plus tard dans ce tutoriel.

1. Cliquez à droite sur le nom du projet (**Wingtip Toys**) dans **Solution Explorer** et **sélectionnez Ajouter un nouveau dossier**. 

    ![Caisse et paiement avec PayPal - Nouveau dossier](checkout-and-payment-with-paypal/_static/image1.png)
2. Nommez le nouveau dossier *Checkout*.
3. Cliquez à droite sur le dossier *Checkout,* puis sélectionnez **Ajouter**-&gt;**un nouvel article**. 

    ![Caisse et paiement avec PayPal - Nouvel article](checkout-and-payment-with-paypal/_static/image2.png)
4. La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
5. Sélectionnez le groupe de modèles **Web Visual** **CMD**  - &gt; sur la gauche. Puis, à partir de la vitre du milieu, sélectionnez **formulaire Web avec Master Page**et nomez-le *CheckoutStart.aspx*. 

    ![Caisse et paiement avec PayPal - Ajouter un nouveau dialogue d’objets](checkout-and-payment-with-paypal/_static/image3.png)
6. Comme avant, sélectionnez le fichier *Site.Master* comme page principale.
7. Ajoutez les pages supplémentaires suivantes au dossier *Checkout* à l’aide des mêmes étapes ci-dessus :   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Ajouter un fichier Web.config

En ajoutant un nouveau fichier *Web.config* au dossier *Checkout,* vous serez en mesure de restreindre l’accès à toutes les pages contenues dans le dossier.

1. Cliquez à droite sur le dossier *Checkout* et sélectionnez **Ajouter**  - &gt; **un nouvel article**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le groupe de modèles **Web Visual** **CMD**  - &gt; sur la gauche. Puis, à partir de la vitre du milieu, sélectionnez **Web Configuration File**, accepter le nom par défaut de *Web.config*, puis sélectionnez **Ajouter**.
3. Remplacez le contenu XML existant dans le fichier *Web.config* par le code suivant :  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Enregistrez le fichier *Web.config*.

Le fichier *Web.config* précise que tous les utilisateurs inconnus de l’application Web doivent se voir refuser l’accès aux pages contenues dans le dossier *Checkout.* Toutefois, si l’utilisateur a enregistré un compte et est connecté, il sera un utilisateur connu et aura accès aux pages dans le dossier *Checkout.*

Il est important de noter que ASP.NET configuration suit une hiérarchie, où chaque fichier *Web.config* applique des paramètres de configuration au dossier dans lequel il est dans et à tous les répertoires pour enfants en dessous.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Activation du protocole SSL pour le projet

 SSL (Secure Sockets Layer) est un protocole défini pour autoriser les serveurs et clients web à communiquer de manière plus sécurisée grâce au chiffrement. Lorsque SSL n'est pas utilisé, les données envoyées entre le client et le serveur peuvent être soumises à la détection des paquets par quiconque dispose d'un accès physique au réseau. En outre, plusieurs schémas d'authentification ne sont pas sécurisés sur le HTTP standard. En particulier, l'authentification de base et l'authentification par formulaires envoient des informations d'identification non chiffrées. Pour être sécurisés, ces schémas d'authentification doivent utiliser SSL. 

1. Dans **Solution Explorer**, cliquez sur le projet **WingtipToys,** puis appuyez sur **F4** pour afficher la fenêtre **Propriétés.**
2. Modifier **SSL** `true`Enabled à .
3. Copiez l’ **URL SSL** afin de pouvoir l’utiliser ultérieurement.   
 L’URL SSL `https://localhost:44300/` sera à moins que vous ayez déjà créé des sites Web SSL (comme indiqué ci-dessous).   
    ![Propriétés du projet](checkout-and-payment-with-paypal/_static/image4.png)
4. Dans **Solution Explorer**, cliquez à droite sur le projet **WingtipToys** et cliquez sur **Propriétés**.
5. Dans l’onglet de gauche, cliquez sur **Web**.
6. Modifiez **l’Url du projet** pour utiliser **l’URL SSL** que vous avez enregistrée plus tôt.   
    ![Propriétés du projet Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Enregistrez la page en appuyant sur **Ctrl+S**.
8. Appuyez sur **Ctrl+F5** pour exécuter l’application. Visual Studio affiche une option qui vous permet d'éviter les avertissements SSL.
9. Cliquez sur **Oui** pour approuver le certificat IIS Express SSL et continuer.   
    ![Détails du certificat IIS Express SSL](checkout-and-payment-with-paypal/_static/image6.png)  
  Un avertissement de sécurité s’affiche.
10. Cliquez sur **Oui** pour installer le certificat sur votre hôte local (localhost).   
    ![Boîte de dialogue Avertissement de sécurité](checkout-and-payment-with-paypal/_static/image7.png)  
  La fenêtre du navigateur s’affiche.

Vous pouvez maintenant facilement tester votre application Web localement à l’aide de SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Ajout d'un fournisseur OAuth 2.0

ASP.NET Formulaires Web offre des options améliorées pour l’adhésion et l’authentification. Ces améliorations incluent OAuth. OAuth est un protocole ouvert qui permet une autorisation sécurisée dans une méthode simple et standard à partir d’applications Web, mobiles et de bureau. Le modèle web ASP.NET utilise OAuth pour exposer Facebook, Twitter, Google et Microsoft en tant que fournisseurs d’authentification. Bien que ce tutoriel utilise uniquement Google comme fournisseur d’authentification, vous pouvez facilement modifier le code pour utiliser l’un des fournisseurs. Les étapes pour mettre en œuvre d’autres fournisseurs sont très similaires aux étapes que vous verrez dans ce tutoriel.

En plus de l’authentification, ce didacticiel va également utiliser des rôles pour l’implémentation d’autorisations. Seuls les utilisateurs que vous ajoutez au rôle `canEdit` pourront créer, modifier ou supprimer des contacts.

> [!NOTE] 
> 
> Les applications Windows Live n’acceptent qu’une URL en direct pour un site Web en fonction, de sorte que vous ne pouvez pas utiliser une URL locale de site Web pour tester les connexions.

Les étapes suivantes vous permettent d'ajouter un fournisseur d'authentification Google.

1. Ouvrez le fichier *App\_Start-Startup.Auth.cs.*
2. Supprimez les caractères de commentaire de la méthode `app.UseGoogleAuthentication()` de telle sorte qu’elle ait l’aspect suivant : 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Accédez à la [Console développeur de Google](https://console.developers.google.com/). Vous devez également vous connecter avec votre compte de messagerie de développeur Google (gmail.com). Si vous n’avez pas de compte Google, sélectionnez le lien **Créer un compte** .   
   Vous accédez alors à **Google Developers Console**.   
    ![Console développeur de Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Cliquez sur le bouton **Créer le projet** et entrer un nom de projet et un ID (vous pouvez utiliser les valeurs par défaut). Ensuite, cliquez sur la **case à cocher de l’accord** et le bouton **Créer.**  

    ![Google - Nouveau projet](checkout-and-payment-with-paypal/_static/image9.png)

    En quelques secondes, le projet est créé et votre navigateur affiche la nouvelle page de projets.
5. Dans l’onglet gauche, cliquez sur **API &amp; auth**, puis cliquez sur **Credentials**.
6. Cliquez sur le **nouveau client ID Créer** sous **OAuth**.   
   La boîte de dialogue **Créer un identifiant client** s’affiche.   
    ![Google - Créer un identifiant client](checkout-and-payment-with-paypal/_static/image10.png)
7. Dans le dialogue **Create Client ID,** conservez l’application **Web** par défaut pour le type d’application.
8. Définissez les **origines JavaScript autorisées** à l’URL`https://localhost:44300/` SSL que vous avez utilisée plus tôt dans ce tutoriel (sauf si vous avez créé d’autres projets SSL).   
   Cette URL est l'origine de votre application. Pour cet exemple, vous entrerez uniquement l'URL de test de l'hôte local (localhost). Cependant, vous pouvez entrer plusieurs URL pour tenir compte de l’ahost local et de la production.
9. Définissez les **URI de redirection autorisés** comme suit : 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Cette valeur est l’URI que le protocole OAuth ASP.NET utilise pour communiquer avec le serveur OAuth Google. N’oubliez pas l’URL `https://localhost:44300/` SSL que vous avez utilisée ci-dessus (sauf si vous avez créé d’autres projets SSL).
10. Cliquez sur le bouton **Créer un identifiant client.**
11. Sur le menu gauche de la console Google Developers, cliquez sur l’élément du menu **d’écran Consentement,** puis réglez votre adresse e-mail et votre nom de produit. Lorsque vous avez terminé le formulaire, cliquez sur **Enregistrer**.
12. Cliquez sur l’élément du menu **API,** faites défiler vers le bas et cliquez sur le bouton **off** à côté de **l’API Google**.   
    L’acceptation de cette option permettra l’API GoogleMD.
13. Vous devez également mettre à jour le paquet **Microsoft.Owin** NuGet à la version 3.0.0.   
    Dans le menu **Tools,** sélectionnez **NuGet Package Manager,** puis **sélectionnez Les forfaits Manage NuGet pour la solution**.  
    De la fenêtre **Manage NuGet Packages,** trouvez et mettez à jour le package **Microsoft.Owin** à la version 3.0.0.
14. Dans Visual Studio, `UseGoogleAuthentication` mettez à jour la méthode de la *Startup.Auth.cs* page en copiant et en collé **l’ID client** et **le secret client** dans la méthode. Les valeurs **d’identification du client** et **de secret client** indiquées ci-dessous sont des échantillons et ne fonctionneront pas. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Appuyez sur **CTRL-F5** pour construire et exécuter l’application. Cliquez sur le lien **Ouvrir une session** .
16. Sous **l’utilisation d’un autre service pour vous connecter,** cliquez sur **Google**.  
    ![Ouvrir une session](checkout-and-payment-with-paypal/_static/image11.png)
17. Si vous devez entrer vos informations d’identification, vous êtes redirigé vers le site Google approprié.  
    ![Google - Se connecter](checkout-and-payment-with-paypal/_static/image12.png)
18. Après avoir saisi vos informations d’identification, vous serez invité à donner des autorisations à l’application Web que vous venez de créer.  
    ![Compte de service de projet par défaut](checkout-and-payment-with-paypal/_static/image13.png)
19. Cliquez sur **Accepter**. Vous serez redirigé vers la page **Registre** de l’application **WingtipToys** où vous pouvez enregistrer votre compte Google.  
    ![S’inscrire avec un compte Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Vous avez la possibilité de changer le nom d'inscription local utilisé pour votre compte Gmail, mais, en règle générale, les utilisateurs préfèrent conserver l'alias de messagerie par défaut (c'est-à-dire celui que vous avez utilisé pour l'authentification). Cliquez **sur Connectez-vous** comme indiqué ci-dessus.

### <a name="modifying-login-functionality"></a>Modification de la fonctionnalité Login

Comme mentionné précédemment dans cette série de tutoriels, une grande partie de la fonctionnalité d’enregistrement de l’utilisateur a été inclus dans le modèle ASP.NET formulaires Web par défaut. Maintenant, vous modifierez les pages Par défaut *Login.aspx* et *Register.aspx* pour appeler la `MigrateCart` méthode. La `MigrateCart` méthode associe un utilisateur nouvellement connecté à un panier anonyme. En associant l’utilisateur et le panier d’achat, l’application d’échantillon Wingtip Toys sera en mesure de maintenir le panier d’achat de l’utilisateur entre les visites.

1. Dans **Solution Explorer**, trouvez et ouvrez le dossier de *compte.*
2. Modifier la page de code-derrière nommé *Login.aspx.cs* d’inclure le code mis en surbrillance en jaune, de sorte qu’il apparaît comme suit:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Enregistrez le *fichier Login.aspx.cs.*

Pour l’instant, vous pouvez ignorer l’avertissement qu’il n’y a pas de définition pour la `MigrateCart` méthode. Vous l’ajouterez un peu plus tard dans ce tutoriel.

Le *fichier Login.aspx.cs* de code-derrière prend en charge une méthode LogIn. En inspectant la page Login.aspx, vous verrez que cette page inclut un bouton `LogIn` " Connectez-vous " qui, lorsque le clic, déclenche le gestionnaire sur le code derrière.

Lorsque `Login` la méthode sur le *Login.aspx.cs* est appelée, une nouvelle `usersShoppingCart` instance du panier nommé est créée. L’ID du panier (un GUID) est récupéré `cartId` et réglé sur la variable. Ensuite, `MigrateCart` la méthode est appelée, en passant à la fois le `cartId` nom et le nom de l’utilisateur connecté à cette méthode. Lorsque le panier est migré, le GUID utilisé pour identifier le panier anonyme est remplacé par le nom d’utilisateur.

En plus de modifier le *fichier Login.aspx.cs* de code pour migrer le panier d’achat lorsque l’utilisateur se connecte, vous devez également modifier le fichier Register.aspx.cs *de code-derrière* pour migrer le panier d’achat lorsque l’utilisateur crée un nouveau compte et se connecte.

1. Dans le dossier *de compte,* ouvrez le fichier de code-derrière nommé *Register.aspx.cs*.
2. Modifier le fichier de code en incluant le code en jaune, de sorte qu’il apparaît comme suit:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Enregistrez le *fichier Register.aspx.cs.* Encore une fois, ignorez l’avertissement sur la `MigrateCart` méthode.

Notez que le code `CreateUser_Click` que vous avez utilisé dans le `LogIn` gestionnaire d’événements est très similaire au code que vous avez utilisé dans la méthode. Lorsque l’utilisateur s’inscrit ou se connecte `MigrateCart` au site, un appel à la méthode sera effectué.

## <a name="migrating-the-shopping-cart"></a>Migrer le panier d’achats

Maintenant que vous avez mis à jour le processus de connexion et d’inscription, vous pouvez ajouter le code pour migrer le panier d’achat en utilisant la `MigrateCart` méthode.

1. Dans **Solution Explorer**, trouvez le dossier *Logic* et ouvrez le fichier de classe *ShoppingCartActions.cs.*
2. Ajoutez le code mis en surbrillance en jaune au code existant dans le fichier *ShoppingCartActions.cs,* de sorte que le code dans le fichier *ShoppingCartActions.cs* apparaît comme suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

La `MigrateCart` méthode utilise le cartId existant pour trouver le panier de l’utilisateur. Ensuite, le code boucle à travers tous les `CartId` articles de panier `CartItem` et remplace la propriété (comme spécifié par le schéma) avec le nom d’utilisateur connecté.

### <a name="updating-the-database-connection"></a>Mise à jour de la connexion de base de données

Si vous suivez ce tutoriel à l’aide de **l’application préconstrute** Wingtip Toys échantillon, vous devez recréer la base de données d’adhésion par défaut. En modifiant la chaîne de connexion par défaut, la base de données d’adhésion sera créée la prochaine fois que l’application s’exécute.

1. Ouvrez le fichier *Web.config* à la racine du projet.
2. Mettre à jour la chaîne de connexion par défaut afin qu’elle apparaisse comme suit :   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Intégration de PayPal

PayPal est une plateforme de facturation basée sur le Web qui accepte les paiements effectués par les marchands en ligne. Ce tutoriel explique ensuite comment intégrer la fonctionnalité Express Checkout de PayPal dans votre application. Express Checkout permet à vos clients d’utiliser PayPal pour payer les articles qu’ils ont ajoutés à leur panier.

### <a name="create-paypal-test-accounts"></a>Créer des comptes de test PayPal

Pour utiliser l’environnement de test PayPal, vous devez créer et vérifier un compte de test développeur. Vous utiliserez le compte de test développeur pour créer un compte de test d’acheteur et un compte de test vendeur. Les informations d’identification du compte de test du développeur permettront également à l’application d’échantillon Wingtip Toys d’accéder à l’environnement de test PayPal.

1. Dans un navigateur, naviguez vers le site de test des développeurs PayPal :   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Si vous n’avez pas de compte de développeur PayPal, créez un nouveau compte en cliquant sur **Sign Up**et en suivant les étapes de l’inscription. Si vous avez un compte de développeur PayPal existant, connectez-vous en cliquant sur **Log In**. Vous aurez besoin de votre compte de développeur PayPal pour tester l’application d’échantillon Wingtip Toys plus tard dans ce tutoriel.
3. Si vous venez de vous inscrire à votre compte de développeur PayPal, vous devrez peut-être vérifier votre compte de développeur PayPal avec PayPal. Vous pouvez vérifier votre compte en suivant les étapes envoyées par PayPal à votre compte de messagerie. Une fois que vous avez vérifié votre compte de développeur PayPal, connectez-vous au site de test du développeur PayPal.
4. Une fois connecté au site de développeur PayPal avec votre compte de développeur PayPal, vous devez créer un compte de test d’acheteur PayPal si vous n’en avez pas déjà. Pour créer un compte de test d’acheteur, sur le site PayPal cliquez sur **l’onglet Applications,** puis cliquez sur **les comptes Sandbox**.   
 La page **des comptes de test Sandbox** est affichée.   

    > [!NOTE] 
    > 
    > Le site PayPal Developer fournit déjà un compte d’essai marchand.

    ![Caisse et paiement avec PayPal - Sandbox comptes de test](checkout-and-payment-with-paypal/_static/image15.png)
5. Sur la page des comptes de test Sandbox, cliquez sur **Create Account**.
6. Sur la page **de compte de test Créer** choisir un e-mail de compte de test d’acheteur et mot de passe de votre choix.   

    > [!NOTE] 
    > 
    > Vous aurez besoin des adresses e-mail de l’acheteur et mot de passe pour tester l’application d’échantillon Wingtip Toys à la fin de ce tutoriel.

    ![Caisse et paiement avec PayPal - Sandbox comptes de test](checkout-and-payment-with-paypal/_static/image16.png)
7. Créez le compte de test de l’acheteur en cliquant sur le bouton **Compte Créer.**  
 La page **des comptes Sandbox Test** est affichée. 

    ![Caisse et paiement avec PayPal - Comptes PayPal](checkout-and-payment-with-paypal/_static/image17.png)
8. Sur la page **des comptes de test Sandbox,** cliquez sur le compte de messagerie **facilitateur.**  
    **Les** options de profil et **de notification** apparaissent.
9. Sélectionnez l’option **Profil,** puis cliquez sur **les informations d’identification API** pour afficher vos informations d’identification API pour le compte de test marchand.
10. Copiez les informations d’identification test API sur bloc-notes.

Vous aurez besoin de vos informations d’identification API Classic TEST (nom d’utilisateur, mot de passe et Signature) pour passer des appels API de l’application Wingtip Toys à l’environnement de test PayPal. Vous ajouterez les informations d’identification dans la prochaine étape.

### <a name="add-paypal-class-and-api-credentials"></a>Ajouter les informations d’identification de la classe PayPal et de l’API

Vous placez la majorité du code PayPal en une seule classe. Cette classe contient les méthodes utilisées pour communiquer avec PayPal. En outre, vous ajouterez vos informations d’identification PayPal à cette classe.

1. Dans l’application d’échantillon Wingtip Toys dans Visual Studio, cliquez à droite sur le dossier **Logic,** puis **sélectionnez Ajouter**  - &gt; **un nouvel article**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sous **le visuel Cmd** de la vitre **installée** sur la gauche, sélectionnez **Code**.
3. De la vitre du milieu, sélectionnez **Classe**. Nommez cette nouvelle classe **PayPalFunctions.cs**.
4. Cliquez sur **Add**.  
   Le nouveau fichier de classe est affiché dans l’éditeur.
5. Remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Ajoutez les informations d’identification De l’API Marchand (nom d’utilisateur, mot de passe et Signature) que vous avez affichées plus tôt dans ce tutoriel afin que vous puissiez passer des appels fonctionnels vers l’environnement de test PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> Dans cette application d’échantillon, vous ajoutez simplement des informations d’identification à un fichier CMD (.cs). Toutefois, dans une solution implémentée, vous devriez envisager de chiffrer vos informations d’identification dans un fichier de configuration.

La classe NVPAPICaller contient la majorité des fonctionnalités PayPal. Le code de la classe fournit les méthodes nécessaires pour effectuer un achat de test dans l’environnement de test PayPal. Les trois fonctions PayPal suivantes sont utilisées pour effectuer des achats :

- Fonction `SetExpressCheckout`
- Fonction `GetExpressCheckoutDetails`
- Fonction `DoExpressCheckoutPayment`

La `ShortcutExpressCheckout` méthode recueille les informations d’achat de test `SetExpressCheckout` et les détails du produit du panier d’achat et appelle la fonction PayPal. La `GetCheckoutDetails` méthode confirme les `GetExpressCheckoutDetails` détails de l’achat et appelle la fonction PayPal avant de faire l’achat du test. La `DoCheckoutPayment` méthode complète l’achat de test `DoExpressCheckoutPayment` de l’environnement de test en appelant la fonction PayPal. Le code restant prend en charge les méthodes et processus PayPal, tels que l’encodage des chaînes, le décodage des chaînes, les tableaux de traitement et la détermination des informations d’identification.

> [!NOTE] 
> 
> PayPal vous permet d’inclure des détails d’achat facultatifs basés sur [les spécifications API de PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). En étendant le code dans l’application d’échantillon Wingtip Toys, vous pouvez inclure des détails de localisation, des descriptions de produits, des taxes, un numéro de service à la clientèle, ainsi que de nombreux autres domaines optionnels.

Notez que les URL de retour et d’annulation spécifiées dans la méthode **ShortcutExpressCheckout** utilisent un numéro de port.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Lorsque Visual Web Developer exécute un projet web à l’aide de SSL, généralement le port 44300 est utilisé pour le serveur Web. Comme indiqué ci-dessus, le numéro de port est de 44300. Lorsque vous exécutez l’application, vous pouvez voir un numéro de port différent. Votre numéro de port doit être correctement défini dans le code afin que vous puissiez exécuter avec succès l’application d’échantillon Wingtip Toys à la fin de ce tutoriel. La section suivante de ce tutoriel explique comment récupérer le numéro de port hôte local et mettre à jour la classe PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Mettre à jour le numéro de port localHost dans la classe PayPal

L’application d’échantillon Wingtip Toys achète des produits en naviguant vers le site d’essai PayPal et en revenant à votre instance locale de l’application d’échantillon Wingtip Toys. Afin d’obtenir le retour de PayPal à l’URL correcte, vous devez spécifier le numéro de port de l’application d’échantillon en cours d’exécution locale dans le code PayPal mentionné ci-dessus.

1. Cliquez à droite sur le nom du projet (**WingtipToys**) dans **Solution Explorer** et **sélectionnez Propriétés**.
2. Dans la colonne de gauche, sélectionnez l’onglet **Web.**
3. Récupérez le numéro de port de la boîte **Url du projet.**
4. Si nécessaire, `returnURL` mettez `cancelURL` à jour le`NVPAPICaller`et dans la classe PayPal ( ) dans le fichier *PayPalFunctions.cs* pour utiliser le numéro de port de votre application web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Maintenant, le code que vous avez ajouté correspondra au port prévu pour votre application Web locale. PayPal sera en mesure de revenir à l’URL correcte sur votre machine locale.

### <a name="add-the-paypal-checkout-button"></a>Ajouter le bouton PayPal Checkout

Maintenant que les principales fonctions PayPal ont été ajoutées à l’application d’échantillon, vous pouvez commencer à ajouter la majoration et le code nécessaires pour appeler ces fonctions. Tout d’abord, vous devez ajouter le bouton de caisse que l’utilisateur verra sur la page panier d’achat.

1. Ouvrez le fichier *ShoppingCart.aspx.*
2. Faites défiler vers le bas `<!--Checkout Placeholder -->` du fichier et trouvez le commentaire.
3. Remplacez le `ImageButton` commentaire par un contrôle de sorte que la marque est remplacée comme suit :  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. Dans le dossier *ShoppingCart.aspx.cs,* après `UpdateBtn_Click` que le gestionnaire de l’événement près de la fin du dossier, ajouter le gestionnaire de l’événement: `CheckOutBtn_Click`  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Également dans le *fichier ShoppingCart.aspx.cs,* ajouter une `CheckoutBtn`référence à la , de sorte que le nouveau bouton d’image est référencé comme suit:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Enregistrez vos modifications à la fois dans le fichier *ShoppingCart.aspx* et dans le fichier *ShoppingCart.aspx.cs.*
7. Parmi le menu, sélectionnez **Debug**-&gt;**Build WingtipToys**.  
   Le projet sera reconstruit avec le nouveau contrôle **ImageButton.**

### <a name="send-purchase-details-to-paypal"></a>Envoyer les détails de l’achat à PayPal

Lorsque l’utilisateur clique sur le bouton **Checkout** sur la page du panier d’achat (*ShoppingCart.aspx*), il commencera le processus d’achat. Le code suivant appelle la première fonction PayPal nécessaire pour acheter des produits.

1. Du dossier *Checkout,* ouvrez le fichier de code-derrière nommé *CheckoutStart.aspx.cs*.   
   Assurez-vous d’ouvrir le fichier de code-derrière.
2. Remplacez le code existant par le code ci-dessous :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Lorsque l’utilisateur de l’application clique sur le bouton **Checkout** sur la page du panier d’achat, le navigateur naviguera vers la page *CheckoutStart.aspx.* Lorsque la page *CheckoutStart.aspx* `ShortcutExpressCheckout` se charge, la méthode est appelée. À ce stade, l’utilisateur est transféré sur le site de test PayPal. Sur le site PayPal, l’utilisateur saisie ses informations d’identification PayPal, passe en revue les détails de `ShortcutExpressCheckout` l’achat, accepte l’accord PayPal et retourne à l’application d’échantillon Wingtip Toys lorsque la méthode se termine. Lorsque `ShortcutExpressCheckout` la méthode est terminée, elle redirigera l’utilisateur vers la `ShortcutExpressCheckout` page *CheckoutReview.aspx* spécifiée dans la méthode. Cela permet à l’utilisateur d’examiner les détails de la commande à partir de l’application Wingtip Toys échantillon.

### <a name="review-order-details"></a>Examen des détails de l’ordre

De retour de PayPal, la page *CheckoutReview.aspx* de l’application d’échantillons Wingtip Toys affiche les détails de la commande. Cette page permet à l’utilisateur d’examiner les détails de la commande avant d’acheter les produits. La page *CheckoutReview.aspx* doit être créée comme suit :

1. Dans le dossier *Checkout,* ouvrez la page nommée *CheckoutReview.aspx*.
2. Remplacer la majoration existante par les éléments suivants :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Ouvrez la page de code-derrière nommée *CheckoutReview.aspx.cs* et remplacez le code existant par ce qui suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Le contrôle **DetailsView** est utilisé pour afficher les détails de commande qui ont été retournés de PayPal. En outre, le code ci-dessus enregistre les détails `OrderDetail` de l’ordre à la base de données Wingtip Toys comme objet. Lorsque l’utilisateur clique sur le bouton **Commande complète,** il est redirigé vers la page *CheckoutComplete.aspx.*

> [!NOTE] 
> 
> **Conseil**
> 
> Dans le balisage de la page *CheckoutReview.aspx,* notez que l’étiquette `<ItemStyle>` est utilisée pour modifier le style des éléments dans le contrôle **DetailsView** près du bas de la page. En visualisant la page de **Design View** (en **sélectionnant** design dans le coin inférieur gauche de Visual Studio), puis en sélectionnant le contrôle **DetailsView,** et en sélectionnant le **Smart Tag** (l’icône de flèche en haut à droite du contrôle), vous pourrez voir les **tâches DetailsView**.
> 
> ![Départ et paiement avec PayPal - Edit Fields](checkout-and-payment-with-paypal/_static/image18.png)
> 
> En sélectionnant **Edit Fields**, la boîte de dialogue **Fields** apparaîtra. Dans cette boîte de dialogue, vous pouvez facilement contrôler les propriétés visuelles, telles que **ItemStyle**, du contrôle **DetailsView.**
> 
> ![Départ et paiement avec PayPal - Fields Dialog](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Achat complet

La page *CheckoutComplete.aspx* effectue l’achat auprès de PayPal. Comme mentionné ci-dessus, l’utilisateur doit cliquer sur le bouton **Complete Order** avant que l’application navigue vers la page *CheckoutComplete.aspx.*

1. Dans le dossier *Checkout,* ouvrez la page nommée *CheckoutComplete.aspx*.
2. Remplacer la majoration existante par les éléments suivants :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Ouvrez la page de code-derrière nommée *CheckoutComplete.aspx.cs* et remplacez le code existant par ce qui suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Lorsque la page *CheckoutComplete.aspx* `DoCheckoutPayment` est chargée, la méthode est appelée. Comme mentionné précédemment, la `DoCheckoutPayment` méthode complète l’achat de l’environnement de test PayPal. Une fois que PayPal a terminé l’achat de la commande, la `ID` page *CheckoutComplete.aspx* affiche une transaction de paiement à l’acheteur.

### <a name="handle-cancel-purchase"></a>Gérer l’achat d’annulation

Si l’utilisateur décide d’annuler l’achat, il sera dirigé vers la page *CheckoutCancel.aspx* où il verra que sa commande a été annulée.

1. Ouvrez la page nommée *CheckoutCancel.aspx* dans le dossier *Checkout.*
2. Remplacer la majoration existante par les éléments suivants :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Gérer les erreurs d’achat

Les erreurs pendant le processus d’achat seront traitées par la page *CheckoutError.aspx.* Le code-derrière de la page *CheckoutStart.aspx,* la page *CheckoutReview.aspx,* et la page *CheckoutComplete.aspx* seront chacun rediriger vers la page *CheckoutError.aspx* si une erreur se produit.

1. Ouvrez la page nommée *CheckoutError.aspx* dans le dossier *Checkout.*
2. Remplacer la majoration existante par les éléments suivants :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

La page *CheckoutError.aspx* est affichée avec les détails de l’erreur lorsqu’une erreur se produit pendant le processus de caisse.

## <a name="running-the-application"></a>Exécution de l'application

Exécutez l’application pour voir comment acheter des produits. Notez que vous serez en cours d’exécution dans l’environnement de test PayPal. Aucun argent réel n’est échangé.

1. Assurez-vous que tous vos fichiers sont enregistrés dans Visual Studio.
2. Ouvrez un navigateur [https://developer.paypal.com](https://developer.paypal.com/)Web et naviguez vers .
3. Connectez-vous avec votre compte de développeur PayPal que vous avez créé plus tôt dans ce tutoriel.  
   Pour le bac à sable développeur de PayPal, [https://developer.paypal.com](https://developer.paypal.com/) vous devez être connecté à la caisse express de test. Cela ne s’applique qu’aux tests de bac à sable de PayPal, et non à l’environnement en direct de PayPal.
4. Dans Visual Studio, appuyez sur **F5** pour exécuter l’application d’échantillon Wingtip Toys.  
   Après la reconstruction de la base de données, le navigateur s’ouvrira et affichera la page *Default.aspx.*
5. Ajoutez trois produits différents au panier en sélectionnant la catégorie de produit, comme "Cars" puis en cliquant **sur Ajouter au panier** à côté de chaque produit.  
   Le panier affichera le produit que vous avez sélectionné.
6. Cliquez sur le bouton **PayPal** pour la caisse. 

    ![Caisse et paiement avec PayPal - Cart](checkout-and-payment-with-paypal/_static/image20.png)

   Pour vérifier, vous aurez besoin d’un compte utilisateur pour l’application d’échantillon Wingtip Toys.
7. Cliquez sur le lien **Google** à droite de la page pour vous connecter à un compte de messagerie gmail.com existant.  
   Si vous n’avez pas de compte gmail.com, vous pouvez en créer un à des fins de test à [www.gmail.com](https://www.gmail.com/). Vous pouvez également utiliser un compte local standard en cliquant sur "Register". 

    ![Caisse et paiement avec PayPal - Connectez-vous](checkout-and-payment-with-paypal/_static/image21.png)
8. Connectez-vous avec votre compte gmail et mot de passe. 

    ![Caisse et paiement avec PayPal - Gmail Sign In](checkout-and-payment-with-paypal/_static/image22.png)
9. Cliquez sur le bouton **Log in** pour enregistrer votre compte gmail avec votre nom d’exemple d’application Wingtip Toys. 

    ![Caisse et paiement avec PayPal - Compte d’enregistrement](checkout-and-payment-with-paypal/_static/image23.png)
10. Sur le site de test PayPal, ajoutez votre adresse e-mail et mot de passe de votre **acheteur** que vous avez créé plus tôt dans ce tutoriel, puis cliquez sur le bouton **Log In.** 

    ![Caisse et paiement avec PayPal - PayPal Sign In](checkout-and-payment-with-paypal/_static/image24.png)
11. Acceptez la politique PayPal et cliquez sur le bouton **Accepter et continuer.**  
    Notez que cette page n’est affichée que la première fois que vous utilisez ce compte PayPal. Encore une fois noter qu’il s’agit d’un compte de test, pas d’argent réel est échangé. 

    ![Départ et paiement avec PayPal - Politique PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Passez en revue les informations de commande sur la page d’examen de l’environnement de test PayPal et cliquez sur **Continuer**. 

    ![Caisse et paiement avec PayPal - Avis d’information](checkout-and-payment-with-paypal/_static/image26.png)
13. Sur la page *CheckoutReview.aspx,* vérifiez le montant de la commande et affichez l’adresse d’expédition générée. Ensuite, cliquez sur le bouton **Commande complète.** 

    ![Caisse et paiement avec PayPal - Examen des commandes](checkout-and-payment-with-paypal/_static/image27.png)
14. La page **CheckoutComplete.aspx** est affichée avec un ID de transaction de paiement. 

    ![Caisse et paiement avec PayPal - Départ complet](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Examen de la base de données

En examinant les données mises à jour dans la base de données d’applications d’échantillons Wingtip Toys après l’exécution de l’application, vous pouvez voir que l’application a enregistré avec succès l’achat des produits.

Vous pouvez inspecter les données contenues dans le fichier de base de données *Wingtiptoys.mdf* en utilisant la fenêtre **Database Explorer** (fenêtre**Server Explorer** dans Visual Studio) comme vous l’avez fait plus tôt dans cette série de tutoriels.

1. Fermez la fenêtre du navigateur si elle est encore ouverte.
2. Dans Visual Studio, sélectionnez l’icône **Afficher tous les fichiers** en haut de Solution **Explorer** pour vous permettre d’élargir le dossier **\_App Data.**
3. Élargissez le dossier **\_App Data.**  
 Vous devrez peut-être sélectionner l’icône **Afficher tous les fichiers** pour le dossier.
4. Cliquez à droite sur le fichier de base de données *Wingtiptoys.mdf* et sélectionnez **Open**.  
    **Server Explorer** s’affiche.
5. Développez le dossier **Tables** .
6. Cliquez à droite sur le tableau **des commandes**et sélectionnez Afficher les données de **la table**.  
 La table **Des commandes** est affichée.
7. Examinez la colonne **PaymentTransactionID** pour confirmer les transactions réussies. 

    ![Caisse et paiement avec PayPal - Base de données d’examen](checkout-and-payment-with-paypal/_static/image29.png)
8. Fermez la fenêtre de la table **Des ordres.**
9. Dans le Server Explorer, cliquez à droite sur la table **OrderDetails** et sélectionnez **Afficher les données de la table**.
10. Examinez `OrderId` `Username` les valeurs et les valeurs dans le tableau **OrderDetails.** Notez que ces `OrderId` `Username` valeurs correspondent aux valeurs et aux valeurs incluses dans le tableau **des commandes.**
11. Fermez la fenêtre de la table **OrderDetails.**
12. Cliquez à droite sur le fichier de base de données Wingtip Toys (*Wingtiptoys.mdf*) et **sélectionnez Close Connection**.
13. Si vous ne voyez pas la fenêtre **Solution Explorer,** cliquez sur **Solution Explorer** au bas de la fenêtre **Server Explorer** pour afficher à nouveau la **solution Explorer.**

## <a name="summary"></a>Récapitulatif

Dans ce tutoriel, vous avez ajouté des schémas de détail de commande et de commande pour suivre l’achat de produits. Vous avez également intégré la fonctionnalité PayPal dans l’application d’échantillon Wingtip Toys.

## <a name="additional-resources"></a>Ressources supplémentaires

[Vue d’ensemble de configuration ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Déployer une application De formulaires Web sécurisées ASP.NET avec adhésion, OAuth et SQL Database au service d’applications Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - Essai gratuit](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Clause d'exclusion de responsabilité

Ce tutoriel contient un exemple de code. Un tel code d’échantillon est fourni « tel quel » sans garantie de quelque nature que ce soit. En conséquence, Microsoft ne garantit pas l’exactitude, l’intégrité ou la qualité du code de l’échantillon. Vous acceptez d’utiliser le code de l’échantillon à vos propres risques. En aucun cas Microsoft ne sera responsable de votre part pour tout code d’échantillon, contenu, y compris, mais non limité, des erreurs ou des omissions dans tout code d’échantillon, contenu, ou toute perte ou dommage de toute nature subie à la suite de l’utilisation de tout code d’échantillon. Vous êtes par la présente notifié et ne par la présente accepter d’indemniser, enregistrer et tenir Microsoft inoffensif de et contre toute perte, réclamations de perte, blessures ou dommages de toute nature, y compris, sans limitation, ceux occasionnés par ou découlant de matériel que vous postez, transmettez, utilisez ou comptez, mais ne se limite pas à, les vues exprimées dans celui-ci.

> [!div class="step-by-step"]
> [Suivant précédent](shopping-cart.md)
> [Next](membership-and-administration.md)
