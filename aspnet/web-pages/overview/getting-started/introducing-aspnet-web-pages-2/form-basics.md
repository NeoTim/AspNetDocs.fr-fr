---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Présentation de ASP.NET pages Web - HTML Formulaire Basics (en anglais seulement) Microsoft Docs
author: Rick-Anderson
description: Ce tutoriel vous montre les bases de la façon de créer un formulaire d’entrée et comment gérer l’entrée de l’utilisateur lorsque vous utilisez ASP.NET Pages Web (Razor). Et maintenant que vous ...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676327"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Présentation de ASP.NET pages Web - HTML Formulaire Basics

 par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce tutoriel vous montre les bases de la façon de créer un formulaire d’entrée et comment gérer l’entrée de l’utilisateur lorsque vous utilisez ASP.NET Pages Web (Razor). Et maintenant que vous avez une base de données, vous utiliserez vos compétences de formulaire pour permettre aux utilisateurs de trouver des films spécifiques dans la base de données. Il suppose que vous avez terminé la série par [introduction à l’affichage des données à l’aide de ASP.NET pages Web](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Ce que vous allez apprendre :
> 
> - Comment créer un formulaire en utilisant des éléments HTML standard.
> - Comment lire l’entrée de l’utilisateur sous une forme.
> - Comment créer une requête SQL qui obtient sélectivement des données en utilisant un terme de recherche que l’utilisateur fournit.
> - Comment avoir des champs dans la page "rappelez-vous" ce que l’utilisateur a entré.
>   
> 
> Caractéristiques/technologies discutées :
> 
> - Objet `Request`.
> - La clause `Where` SQL.

## <a name="what-youll-build"></a>Contenu

Dans le tutoriel précédent, vous avez créé une base `WebGrid` de données, ajouté des données à elle, puis utilisé l’aide pour afficher les données. Dans ce tutoriel, vous ajouterez une boîte de recherche qui vous permet de trouver des films d’un genre spécifique ou dont le titre contient n’importe quel mot que vous entrez. (Par exemple, vous serez en mesure de trouver tous les films dont le genre est "Action" ou dont le titre contient "Harry" ou "Aventure.")

Lorsque vous avez terminé avec ce tutoriel, vous aurez une page comme celle-ci:

![Page de films avec la recherche de genre et de titre](form-basics/_static/image1.png)

La partie de liste de la page &mdash; est la même que dans le dernier tutoriel une grille. La différence sera que la grille ne montrera que les films que vous avez recherchés.

## <a name="about-html-forms"></a>À propos des formulaires HTML

(Si vous avez de l’expérience avec la `GET` `POST`création de formulaires HTML et avec la différence entre et , vous pouvez sauter cette section.)

Un formulaire comporte &mdash; des boîtes de texte, des boutons, des boutons radio, des cases à cocher, des listes de dépôt, etc. Les utilisateurs remplissent ces contrôles ou effectuent des sélections, puis soumettent le formulaire en cliquant sur un bouton.

La syntaxe HTML de base d’une forme est illustrée par cet exemple :

[!code-html[Main](form-basics/samples/sample1.html)]

Lorsque cette balisage s’exécute dans une page, il crée une forme simple qui ressemble à cette illustration:

![Formulaire HTML de base tel qu’il est rendu dans le navigateur](form-basics/_static/image2.png)

L’élément `<form>` contient des éléments HTML à soumettre. (Une erreur facile à faire est d’ajouter des éléments `<form>` à la page, mais ensuite oublier de les mettre à l’intérieur d’un élément. Dans ce cas, rien n’est soumis.) L’attribut `method` indique au navigateur comment soumettre l’entrée de l’utilisateur. Vous définissez `post` ceci si vous effectuez une `get` mise à jour sur le serveur ou si vous êtes juste aller chercher des données à partir du serveur.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST et HTTP Verb Safety**
> 
> HTTP, le protocole que les navigateurs et les serveurs utilisent pour échanger des informations, est remarquablement simple dans ses opérations de base. Les navigateurs n’utilisent que quelques verbes pour faire des demandes aux serveurs. Lorsque vous écrivez du code pour le web, il est utile de comprendre ces verbes et comment le navigateur et le serveur les utilisent. De loin, les verbes les plus couramment utilisés sont les plus :
> 
> - `GET`. Le navigateur utilise ce verbe pour aller chercher quelque chose sur le serveur. Par exemple, lorsque vous tapez une URL dans `GET` votre navigateur, le navigateur effectue une opération pour demander la page que vous souhaitez. Si la page comprend des graphiques, le navigateur effectue des opérations supplémentaires `GET` pour obtenir les images. Si `GET` l’opération doit transmettre des informations au serveur, les informations sont transmises dans le cadre de l’URL dans la chaîne de requête.
> - `POST`. Le navigateur `POST` envoie une demande afin de soumettre des données à ajouter ou à modifier sur le serveur. Par exemple, `POST` le verbe est utilisé pour créer des enregistrements dans une base de données ou modifier ceux qui existent déjà. La plupart du temps, lorsque vous remplissez un formulaire et `POST` cliquez sur le bouton soumettre, le navigateur effectue une opération. Dans `POST` une opération, les données transmises au serveur sont dans le corps de la page.
> 
> Une distinction importante entre ces `GET` verbes est qu’une opération n’est pas censée changer quoi `GET` que ce soit sur le serveur - ou de le mettre d’une manière un peu plus abstraite, une opération n’entraîne pas un changement d’état sur le serveur. Vous pouvez `GET` effectuer une opération sur les mêmes ressources que vous le souhaitez, et ces ressources ne changent pas. (Une `GET` opération est souvent dite « sûre », ou pour utiliser un terme technique, est *idempotent*.) En revanche, bien `POST` sûr, une demande change quelque chose sur le serveur chaque fois que vous effectuez l’opération.
> 
> Deux exemples aideront à illustrer cette distinction. Lorsque vous effectuez une recherche à l’aide d’un moteur comme Bing ou Google, vous remplissez un formulaire qui se compose d’une boîte de texte, puis vous cliquez sur le bouton de recherche. Le navigateur effectue `GET` une opération, avec la valeur que vous avez entré dans la boîte passée dans le cadre de l’URL. L’utilisation d’une `GET` opération pour ce type de formulaire est très bien, parce qu’une opération de recherche ne change pas de ressources sur le serveur, il suffit de chercher des informations.
> 
> Maintenant, considérez le processus de commande de quelque chose en ligne. Vous remplissez les détails de la commande, puis cliquez sur le bouton soumettre. Cette opération sera `POST` une demande, car l’opération entraînera des changements sur le serveur, tels qu’un nouvel enregistrement de commande, un changement dans les informations de votre compte, et peut-être beaucoup d’autres modifications. Contrairement `GET` à l’opération, `POST` vous ne pouvez pas répéter votre demande — si vous le faisiez, chaque fois que vous avez soumis à nouveau la demande, vous généreriez une nouvelle commande sur le serveur. (Dans des cas comme celui-ci, les sites Web vous avertissent souvent de ne pas cliquer sur un bouton de soumission plus d’une fois, ou désactiver le bouton soumettre de sorte que vous ne soumettez pas à nouveau le formulaire accidentellement.)
> 
> Au cours de ce tutoriel, vous `GET` utiliserez `POST` à la fois une opération et une opération pour travailler avec des formulaires HTML. Nous expliquerons dans chaque cas pourquoi le verbe que vous utilisez est le bon.
> 
> (Pour en savoir plus sur les verbes HTTP, voir l’article [De Method Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) sur le site W3C.)

La plupart des `<input>` éléments d’entrée utilisateur sont des éléments HTML. Ils ressemblent `<input type="type" name="name">,` à l’endroit où *le type* indique le type de contrôle des entrées utilisateur que vous voulez. Ces éléments sont les communs :

- Boîte de texte:`<input type="text">`
- Boîte à cocher:`<input type="check">`
- Bouton radio:`<input type="radio">`
- Bouton:`<input type="button">`
- Soumettre le bouton:`<input type="submit">`

Vous pouvez également `<textarea>` utiliser l’élément pour créer `<select>` une boîte de texte multilinline et l’élément pour créer une liste d’abandon ou une liste défilementable. (Pour en savoir plus sur les éléments de formulaire HTML, voir [HTML Forms and Input](http://www.w3schools.com/html/html_forms.asp) sur le site W3Schools.)

L’attribut `name` est très important, parce que le nom est la façon dont vous obtiendrez la valeur de l’élément plus tard, comme vous le verrez sous peu.

La partie intéressante est ce que vous, le développeur de page, faire avec l’entrée de l’utilisateur. Il n’y a pas de comportement intégré associé à ces éléments. Au lieu de cela, vous devez obtenir les valeurs que l’utilisateur a entré ou sélectionné et faire quelque chose avec eux. C’est ce que vous apprendrez dans ce tutoriel.

> [!TIP] 
> 
> **HTML5 et formulaires d’entrée**
> 
> Comme vous le savez peut-être, HTML est en transition et la dernière version (HTML5) inclut la prise en charge pour des moyens plus intuitifs pour les utilisateurs d’entrer des informations. Par exemple, dans HTML5, vous (le développeur de la page) pouvez indiquer à la page que vous souhaitez que l’utilisateur entre une date. Le navigateur peut alors afficher automatiquement un calendrier plutôt que d’exiger de l’utilisateur qu’il saisise une date manuellement. Cependant, HTML5 est nouveau et n’est pas pris en charge dans tous les navigateurs encore.
> 
> ASP.NET Pages Web prend en charge l’entrée HTML5 dans la mesure où le navigateur de l’utilisateur le fait. Pour une idée des nouveaux `<input>` attributs pour l’élément dans HTML5, voir [attribut de type d’entrée &lt;&gt; HTML](http://www.w3schools.com/html/html_form_input_types.asp) sur le site W3Schools.

## <a name="creating-the-form"></a>Création du formulaire

Dans WebMatrix, dans l’espace de travail **Files,** ouvrez la page *Movies.cshtml.*

Après l’étiquette de `</h1>` `<div>` clôture et `grid.GetHtml` avant l’étiquette d’ouverture de l’appel, ajoutez la majoration suivante :

[!code-html[Main](form-basics/samples/sample2.html)]

Cette balisage crée un formulaire `searchGenre` qui a une boîte de texte nommée et un bouton de soumission. La boîte de texte et le `<form>` bouton `method` soumettre sont `get`inclus dans un élément dont l’attribut est défini à . (Rappelez-vous que si vous ne mettez pas `<form>` la boîte de texte et de soumettre le bouton à l’intérieur d’un élément, rien ne sera soumis lorsque vous cliquez sur le bouton.) Vous utilisez `GET` le verbe ici parce que vous créez un formulaire qui n’apporte pas de modifications sur le serveur - il se traduit juste par une recherche. (Dans le tutoriel précédent, `post` vous avez utilisé une méthode, qui est la façon dont vous soumettez des modifications au serveur. Vous verrez que dans le prochain tutoriel à nouveau.)

Exécutez la page. Bien que vous n’ayez défini aucun comportement pour le formulaire, vous pouvez voir à quoi il ressemble :

![Page de films avec boîte de recherche pour Genre](form-basics/_static/image3.png)

Entrez une valeur dans la boîte à texte, comme "Comédie". Cliquez ensuite sur **Search Genre**.

Prenez note de l’URL de la page. Parce que `<form>` vous définissez l’attribut de `method` l’élément à `get`, la valeur que vous avez saisie fait maintenant partie de la chaîne de requête dans l’URL, comme ceci:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Valeurs de forme de lecture

La page contient déjà du code qui obtient des données de base de données et affiche les résultats dans une grille. Maintenant, vous devez ajouter un code qui lit la valeur de la boîte de texte afin que vous puissiez exécuter une requête SQL qui comprend le terme de recherche.

Parce que vous définissez `get`la méthode du formulaire à , vous pouvez lire la valeur qui a été entré dans la boîte de texte en utilisant le code comme ce qui suit:

`var searchTerm = Request.QueryString["searchGenre"];`

L’objet `Request.QueryString` `QueryString` (la `Request` propriété de l’objet) comprend les valeurs `GET` des éléments qui ont été soumis dans le cadre de l’opération. La `Request.QueryString` propriété contient une *collection* (une liste) des valeurs qui sont soumises sous la forme. Pour obtenir une valeur individuelle, vous spécifiez le nom de l’élément que vous voulez. C’est pourquoi vous devez `name` avoir `<input>` un`searchTerm`attribut sur l’élément ( ) qui crée la boîte de texte. (Pour en `Request` savoir plus sur l’objet, voir la [barre latérale](#BKMK_TheRequestObject) plus tard.)

Il est assez simple de lire la valeur de la boîte à texte. Mais si l’utilisateur n’a pas entrer quoi que ce soit du tout dans la boîte de texte, mais a cliqué **Sur la recherche** de toute façon, vous pouvez ignorer ce clic, car il n’y a rien à rechercher.

Le code suivant est un exemple qui montre comment implémenter ces conditions. (Vous n’avez pas encore à ajouter ce code; vous le ferez dans un instant.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Le test se décompose de cette façon:

- Obtenez la `Request.QueryString["searchGenre"]`valeur de , à savoir `<input>` la `searchGenre`valeur qui a été entré dans l’élément nommé .
- Découvrez s’il est vide `IsEmpty` en utilisant la méthode. Cette méthode est le moyen standard de déterminer si quelque chose (par exemple, un élément de forme) contient une valeur. Mais vraiment, vous ne vous souciez que si elle n’est *pas* vide, donc ...
- Ajouter `!` l’opérateur devant `IsEmpty` le test. (L’opérateur `!` signifie logique PAS).

En clair, toute `if` la condition se traduit par ce qui suit: *Si l’élément searchGenre du formulaire n’est pas vide, alors ...*

Ce bloc prépare le terrain pour créer une requête qui utilise le terme de recherche. Vous le ferez dans la section suivante.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **L’objet de demande**
> 
> L’objet `Request` contient toutes les informations que le navigateur envoie à votre application lorsqu’une page est demandée ou soumise. Cet objet comprend toutes les informations que l’utilisateur fournit, comme les valeurs de boîte de texte ou un fichier à télécharger. Il comprend également toutes sortes d’informations supplémentaires, comme les cookies, les valeurs dans la chaîne de requête URL (le cas échéant), le chemin de fichier de la page qui est en cours d’exécution, le type de navigateur que l’utilisateur utilise, la liste des langues qui sont définies dans le navigateur, et bien plus encore.
> 
> L’objet `Request` est une *collection* (liste) de valeurs. Vous obtenez une valeur individuelle de la collection en spécifiant son nom :
> 
> `var someValue = Request["name"];`
> 
> L’objet `Request` expose en fait plusieurs sous-ensembles. Par exemple :
> 
> - `Request.Form`vous donne des valeurs `<form>` à partir d’éléments à l’intérieur de l’élément soumis si la demande est une `POST` demande.
> - `Request.QueryString`vous donne juste les valeurs dans la chaîne de requête de l’URL. (Dans une `http://mysite/myapp/page?searchGenre=action&page=2`URL `?searchGenre=action&page=2` comme, la section de l’URL est la chaîne de requête.)
> - `Request.Cookies`collection vous donne accès aux cookies que le navigateur a envoyés.
> 
> Pour obtenir une valeur que vous savez est `Request["name"]`dans le formulaire soumis, vous pouvez utiliser . Vous `Request.Form["name"]` pouvez également utiliser les versions `POST` plus spécifiques `Request.QueryString["name"]` (pour les demandes) ou (pour `GET` les demandes). Bien sûr, le *nom* est le nom de l’élément à obtenir.
> 
> Le nom de l’article que vous voulez obtenir doit être unique dans la collection que vous utilisez. C’est pourquoi `Request` l’objet fournit `Request.Form` les `Request.QueryString`sous-ensembles comme et . Supposons que votre page `userName` contient un élément `userName`de formulaire nommé et contient *également* un cookie nommé . Si vous `Request["userName"]`obtenez , il est ambigu si vous voulez la valeur du formulaire ou le cookie. Cependant, si `Request.Form["userName"]` vous `Request.Cookie["userName"]`obtenez ou , vous êtes explicite sur la valeur à obtenir.
> 
> C’est une bonne pratique d’être spécifique `Request` et d’utiliser le `Request.Form` `Request.QueryString`sous-ensemble de ce qui vous intéresse, comme ou . Pour les pages simples que vous créez dans ce tutoriel, il ne fait probablement pas vraiment de différence. Cependant, lorsque vous créez des pages `Request.Form` plus `Request.QueryString` complexes, en utilisant la version explicite ou peut vous aider à éviter les problèmes qui peuvent survenir lorsque la page contient un formulaire (ou plusieurs formes), des cookies, des valeurs de chaîne de requête, et ainsi de suite.

## <a name="creating-a-query-by-using-a-search-term"></a>Création d’une requête en utilisant un terme de recherche

Maintenant que vous savez comment obtenir le terme de recherche que l’utilisateur a saisi, vous pouvez créer une requête qui l’utilise. N’oubliez pas que pour sortir tous les éléments du film de la base de données, vous utilisez une requête SQL qui ressemble à cette affirmation:

`SELECT * FROM Movies`

Pour obtenir seulement certains films, vous devez utiliser `Where` une requête qui comprend une clause. Cette clause vous permet de définir une condition sur laquelle les lignes sont retournées par la requête. Voici un exemple :

`SELECT * FROM Movies WHERE Genre = 'Action'`

Le format `WHERE column = value`de base est . Vous pouvez utiliser différents `=`opérateurs `>` en plus `<` juste, comme `<>` (plus grand `<=` que), (moins de), (pas égal à), (moins ou égal à), etc, en fonction de ce que vous cherchez.

Dans le cas où vous vous demandez, les déclarations SQL ne sont pas sensibles &mdash; `SELECT` au cas est le même que `Select` (ou même `select`). Cependant, les gens capitalisent souvent les mots `SELECT` `WHERE`clés dans une déclaration SQL, comme et , pour le rendre plus facile à lire.

### <a name="passing-the-search-term-as-a-parameter"></a>Passer le terme de recherche comme paramètre

La recherche d’un genre`WHERE Genre = 'Action'`spécifique est assez facile (), mais vous voulez être en mesure de rechercher n’importe quel genre que l’utilisateur entre. Pour ce faire, vous créez comme requête SQL qui comprend un placeholder pour la valeur de la recherche. Il ressemblera à cette commande:

`SELECT * FROM Movies WHERE Genre = @0`

Le placeholder `@` est le personnage suivi de zéro. Comme vous pouvez le deviner, une requête peut contenir plusieurs `@0`placesholders, et ils seraient nommés , `@1`, , `@2`etc.

Pour configurer la requête et la transmettre réellement, vous utilisez le code comme suit :

[!code-sql[Main](form-basics/samples/sample4.sql)]

Ce code est similaire à ce que vous avez déjà fait pour afficher les données dans la grille. Les seules différences sont les :

- La requête contient un`WHERE Genre = @0"`placeholder ( ).
- La requête est mise dans`selectCommand`une variable ( ); avant, vous avez passé la `db.Query` requête directement à la méthode.
- Lorsque vous `db.Query` appelez la méthode, vous passez à la fois la requête et la valeur à utiliser pour le placeholder. (Si la requête avait plusieurs placesholders, vous les passeriez tous comme valeurs distinctes à la méthode.)

Si vous mettez tous ces éléments ensemble, vous obtenez le code suivant:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Important!** L’utilisation de `@0`placesholders (comme) pour transmettre des valeurs à une commande SQL est *extrêmement importante* pour la sécurité. La façon dont vous le voyez ici, avec les espaces pour les données variables, est la seule façon de construire des commandes SQL.
> 
> Ne jamais construire une déclaration SQL en mettant en place (concatenating) texte littéral et les valeurs que vous obtenez de l’utilisateur. La souillion de l’entrée de l’utilisateur dans une déclaration SQL ouvre votre site à une *attaque d’injection SQL* où un utilisateur malveillant soumet des valeurs à votre page qui piratent votre base de données. (Vous pouvez en savoir plus dans l’article [SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) le site MSDN.)

## <a name="updating-the-movies-page-with-search-code"></a>Mise à jour de la page Films avec code de recherche

Maintenant, vous pouvez mettre à jour le code dans le fichier *Movies.cshtml.* Pour commencer, remplacez le code dans le bloc de code en haut de la page par ce code :

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La différence ici est que vous avez `selectCommand` mis la requête dans la `db.Query` variable, que vous passerez à plus tard. Mettre la déclaration SQL dans une variable vous permet de modifier l’instruction, qui est ce que vous ferez pour effectuer la recherche.

Vous avez également supprimé ces deux lignes, que vous restituerz plus tard :

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Vous ne voulez pas exécuter la requête encore `db.Query`(c’est-à-dire, appelez `WebGrid` ) et vous ne voulez pas initialiser l’aide encore non plus. Vous ferez ces choses après avoir compris quelle déclaration SQL doit fonctionner.

Après ce bloc réécrit, vous pouvez ajouter la nouvelle logique pour gérer la recherche. Le code terminé ressemblera à ce qui suit. Mettez à jour le code dans votre page afin qu’il corresponde à cet exemple:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La page fonctionne maintenant comme ça. Chaque fois que la page s’exécute, le code ouvre la base de données et la `selectCommand` variable est définie à la déclaration SQL qui obtient tous les enregistrements de la `Movies` table. Le code est également `searchTerm` para initialisé la variable.

Toutefois, si la demande actuelle `searchGenre` inclut une valeur `selectCommand` pour l’élément, le code s’établit à une question différente — à savoir, à une question qui inclut la `Where` clause de recherche d’un genre. Il se `searchTerm` fixe également à tout ce qui a été passé pour la boîte de recherche (qui pourrait être rien).

Indépendamment de quelle déclaration `selectCommand`SQL est `db.Query` en , le code appelle alors `searchTerm`à exécuter la requête, en le passant tout ce qui est en . S’il n’y a rien dans `searchTerm`, il n’a pas d’importance, `selectCommand` parce que dans ce cas, il n’y a pas de paramètre pour passer la valeur à de toute façon.

Enfin, le code initialise l’aide `WebGrid` en utilisant les résultats de la requête, comme avant.

Vous pouvez voir qu’en mettant la déclaration SQL et le terme de recherche dans les variables, vous avez ajouté de la flexibilité au code. Comme vous le verrez plus tard dans ce tutoriel, vous pouvez utiliser ce cadre de base et continuer à ajouter la logique pour différents types de recherches.

## <a name="testing-the-search-by-genre-feature"></a>Test de la fonction Recherche par genre

Dans WebMatrix, exécutez la page *Movies.cshtml.* Vous voyez la page avec la boîte de texte pour le genre.

Entrez un genre que vous avez entré pour l’un de vos enregistrements de test, puis cliquez sur **Search**. Cette fois, vous voyez une liste de films qui correspondent à ce genre:

![Liste de pages de films après la recherche de genre 'Comedies'](form-basics/_static/image4.png)

Entrez un genre différent et recherchez à nouveau. Essayez d’entrer dans le genre en utilisant toutes les majuscules ou toutes les lettres majuscules afin que vous puissiez voir que la recherche n’est pas sensible aux cas.

## <a name="remembering-what-the-user-entered"></a>"Se souvenir" de ce que l’utilisateur est entré

Vous avez peut-être remarqué qu’après avoir entré un genre et cliqué **Search Genre**, vous avez vu une liste pour ce genre. Cependant, la boîte de &mdash; texte de recherche était vide en d’autres termes, il ne se souvenait pas de ce que vous aviez entré.

Il est important de comprendre pourquoi ce comportement se produit. Lorsque vous soumettez une page, le navigateur envoie une demande au serveur Web. Lorsque ASP.NET reçoit la demande, il crée une toute nouvelle instance de la page, exécute le code en elle, puis rend la page sur le navigateur à nouveau. En effet, cependant, la page ne sait pas que vous étiez juste travailler avec une version précédente de lui-même. Tout ce qu’il sait, c’est qu’il a obtenu une demande qui avait des données de forme en elle.

Chaque fois que &mdash; vous demandez une page que &mdash; ce soit pour la première fois ou en la soumettant, vous obtenez une nouvelle page. Le serveur web n’a aucun souvenir de votre dernière demande. Pas plus que ASP.NET, et le navigateur non plus. Le seul lien entre ces instances distinctes de la page est toutes les données que vous transmettez entre eux. Si vous soumettez une page, par exemple, la nouvelle instance de page peut obtenir les données de formulaire qui ont été envoyées par l’instance antérieure. (Une autre façon de transmettre des données entre les pages est d’utiliser des cookies.)

Une façon formelle de décrire cette situation est de dire que les pages Web sont *apatrides*. Les serveurs Web et les pages elles-mêmes et les éléments de la page ne conservent aucune information sur l’état précédent d’une page. Le Web a été conçu de cette façon parce que le maintien de l’état pour les demandes individuelles serait rapidement épuiser les ressources des serveurs Web, qui traitent souvent des milliers, peut-être même des centaines de milliers, de demandes par seconde.

C’est pourquoi la boîte à texte était vide. Après avoir soumis la page, ASP.NET créé une nouvelle instance de la page et a couru à travers le code et le balisage. Il n’y avait rien dans ce code qui ASP.NET de mettre une valeur dans la boîte à texte. Donc, ASP.NET n’a rien fait, et la boîte de texte a été rendue sans une valeur en elle.

Il ya en fait un moyen facile de contourner cette question. Le genre que vous avez entré dans la &mdash; boîte de `Request.QueryString["searchGenre"]`texte *est* à votre disposition dans le code, il est en .

Mettre à jour le balisage `value` pour la `searchTerm`boîte de texte de sorte que l’attribut obtient sa valeur de , comme cet exemple:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Dans cette page, vous auriez `value` également `searchTerm` pu définir l’attribut à la variable, puisque cette variable contient également le genre que vous avez entré. Mais l’utilisation de l’objet `Request` pour définir l’attribut `value` tel qu’il est indiqué ici est la façon standard d’accomplir cette tâche. (En supposant que &mdash; vous voulez même le faire dans certaines situations, vous voudrez peut-être rendre la page *sans* valeurs dans les champs. Tout dépend de ce qui se passe avec votre application.)

> [!NOTE]
> Vous ne pouvez pas « se souvenir » de la valeur d’une boîte de texte utilisée pour les mots de passe. Ce serait un trou de sécurité pour permettre aux gens de remplir un champ de mot de passe en utilisant le code.

Exécutez la page à nouveau, entrez un genre, et cliquez sur **Search Genre**. Cette fois, non seulement vous voyez les résultats de la recherche, mais la boîte de texte se souvient de ce que vous avez entré la dernière fois:

![Page montrant que la boîte de texte a «rappelé» l’entrée précédente](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Recherche de n’importe quel mot dans le titre

Vous pouvez maintenant rechercher n’importe quel genre, mais vous voudrez peut-être aussi chercher un titre. Il est difficile d’obtenir un titre exactement juste lorsque vous recherchez, donc au lieu de cela vous pouvez rechercher un mot qui apparaît n’importe où à l’intérieur d’un titre. Pour ce faire dans SQL, vous utilisez l’opérateur et la `LIKE` syntaxe comme suit :

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Cette commande obtient tous les films dont les titres contiennent "aventure". Lorsque vous `LIKE` utilisez l’opérateur, vous `%` incluez le caractère wildcard dans le cadre du terme de recherche. La `LIKE 'adventure%'` recherche signifie "commencer par 'aventure'". (Techniquement, cela signifie «La chaîne «aventure» suivie de quoi que ce soit.") De même, `LIKE '%adventure'` le terme de recherche signifie « tout ce qui est suivi par la chaîne « aventure », qui est une autre façon de dire « se terminant par « l’aventure ».

Le terme `LIKE '%adventure%'` de recherche signifie donc «avec «aventure» n’importe où dans le titre. (Techniquement, "tout dans le titre, suivi par 'aventure', suivi de quoi que ce soit.")

À `<form>` l’intérieur de l’élément, `</div>` ajoutez le balisage suivant `</form>` sous l’étiquette de clôture pour la recherche de genre (juste avant l’élément de clôture) :

[!code-html[Main](form-basics/samples/sample10.html)]

Le code pour gérer cette recherche est similaire au code pour la `LIKE` recherche de genre, sauf que vous devez assembler la recherche. À l’intérieur du bloc de code `if` en haut `if` de la page, ajoutez ce bloc juste après le bloc pour la recherche de genre:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Ce code utilise la même logique que vous `LIKE` avez vu plus`%`tôt, sauf que la recherche utilise un opérateur et le code met " avant et après le terme de recherche.

Remarquez comment il était facile d’ajouter une autre recherche à la page. Tout ce que tu avais à faire, c’était :

- Créez `if` un bloc testé pour voir si la boîte de recherche pertinente avait une valeur.
- Définissez `selectCommand` la variable à un nouveau relevé SQL.
- Définissez `searchTerm` la variable à la valeur pour passer à la requête.

Voici le bloc de code complet, qui contient la nouvelle logique pour une recherche de titre:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Voici un résumé de ce que fait ce code :

- Les `searchTerm` variables `selectCommand` et sont parasécés en haut. Vous allez définir ces variables au terme de recherche approprié (le cas échéant) et à la commande SQL appropriée en fonction de ce que l’utilisateur fait dans la page. La recherche par défaut est le cas simple d’obtenir tous les films de la base de données.
- Dans les `searchGenre` tests `searchTitle`pour et `searchTerm` , le code définit à la valeur que vous souhaitez rechercher. Ces blocs de `selectCommand` code ont également réglé à une commande SQL appropriée pour cette recherche.
- La `db.Query` méthode n’est invoquée qu’une `selectedCommand` seule fois, `searchTerm`en utilisant n’importe quelle commande SQL est en et quelle que soit la valeur est en . S’il n’y a pas de terme de `searchTerm` recherche (pas de genre et pas de mot-titre), la valeur de est une chaîne vide. Cependant, cela n’a pas d’importance, parce que dans ce cas, la requête ne nécessite pas un paramètre.

## <a name="testing-the-title-search-feature"></a>Test de la fonction de recherche de titres

Vous pouvez maintenant tester votre page de recherche terminée. Exécuter *Movies.cshtml*.

Entrez un genre et cliquez sur **Search Genre**. La grille affiche des films de ce genre, comme avant.

Entrez un mot de titre et cliquez sur **titre de recherche**. La grille affiche des films qui ont ce mot dans le titre.

![Liste de page de films après la recherche de «The» dans le titre](form-basics/_static/image6.png)

Laissez les deux boîtes de texte vides et cliquez sur l’un ou l’autre bouton. La grille affiche tous les films.

## <a name="combining-the-queries"></a>Combiner les requêtes

Vous remarquerez peut-être que les recherches que vous pouvez effectuer sont exclusives. Vous ne pouvez pas rechercher le titre et le genre en même temps, même si les deux boîtes de recherche ont des valeurs en eux. Par exemple, vous ne pouvez pas rechercher tous les films d’action dont le titre contient "Aventure". (Comme la page est codée maintenant, si vous entrez des valeurs pour le genre et le titre, la recherche de titre obtient la priorité.) Pour créer une recherche qui combine les conditions, vous devrez créer une requête SQL qui a la syntaxe comme ce qui suit:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Et vous auriez à exécuter la requête en utilisant une déclaration comme ce qui suit (grossièrement parlant):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Créer une logique pour permettre de nombreuses permutations de critères de recherche peut s’impliquer un peu, comme vous pouvez le voir. Par conséquent, nous allons nous arrêter ici.

## <a name="coming-up-next"></a>Prochaine étape

Dans le tutoriel suivant, vous allez créer une page qui utilise un formulaire pour permettre aux utilisateurs d’ajouter des films à la base de données.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Liste complète pour la page du film (mise à jour avec la recherche)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de la programmation Web ASP.NET à l'aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL WHERE Clause](http://www.w3schools.com/sql/sql_where.asp) sur le site W3Schools
- [Article de Définitions de](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) méthode sur le site W3C

> [!div class="step-by-step"]
> [Suivant précédent](displaying-data.md)
> [Next](entering-data.md)
