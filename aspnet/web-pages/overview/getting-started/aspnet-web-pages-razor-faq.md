---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Pages Web (Razor) FAQ (fr) Microsoft Docs
author: Rick-Anderson
description: Cet article énumère quelques questions fréquemment posées sur ASP.NET Pages Web (Razor) et WebMatrix. Versions logicielles utilisées dans le tutoriel ASP.NET Pages Web (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: a312d1327bc88e721bf7fc7459e420e3f582c88d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543702"
---
# <a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages - Questions fréquentes (FAQ) (Razor)

 par [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix n’est plus recommandé comme environnement de développement intégré pour ASP.NET pages Web. Utilisez [Visual Studio](xref:web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) ou Visual Studio [Code](https://code.visualstudio.com/).
>
> Cet article énumère quelques questions fréquemment posées sur ASP.NET Pages Web (Razor) et WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le tutoriel
> 
> 
> - ASP.NET Pages Web (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Ce tutoriel fonctionne également avec ASP.NET Pages Web 2, WebMatrix 2 et Visual Studio 2012.

- [Quelle est la différence entre ASP.NET pages Web, ASP.NET formulaires Web et ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Ai-je besoin de WebMatrix pour travailler avec Les pages Web ?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Puis-je utiliser ASP.NET les contrôles Web de formulaires sur une page Web Pages ?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Puis-je déployer un site de pages Web ASP.NET sans utiliser WebMatrix ?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Dois-je utiliser l’aide WebSecurity pour prendre en charge les connexions ?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Est-ce ASP.NET Pages Web prend en charge HTML5 ?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Puis-je utiliser JavaScript et jQuery avec des pages Web ?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Ressources supplémentaires](#AdditionalResources)

Pour les questions sur les erreurs et autres questions, consultez le [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Quelle est la différence entre ASP.NET pages Web, ASP.NET formulaires Web et ASP.NET MVC?

Tous les trois sont des technologies ASP.NET pour créer des applications web dynamiques :

- ASP.NET Pages Web se concentre sur l’ajout d’un code dynamique (côté serveur) et l’accès à la base de données pour les pages HTML, et dispose d’une syntaxe simple et légère.
- ASP.NET Web Forms est basé sur un modèle d’objet de page et des commandes traditionnelles de type fenêtre (boutons, listes, etc.). Web Forms utilise un modèle basé sur les événements qui est familier à ceux qui ont travaillé avec le développement basé sur le client (formulaires Windows).
- ASP.NET MVC implémente le modèle de contrôleur de vue modèle pour ASP.NET. L’accent est mis sur la « séparation des préoccupations » (traitement, données et couches d’interface utilisateur).

Les trois cadres sont entièrement soutenus et continuent d’être élaborés par l’équipe ASP.NET. En général, le choix du cadre à utiliser dépend de vos antécédents et de votre expérience avec ASP.NET.

ASP.NET Pages Web en particulier a été conçu pour faciliter la tâche des personnes qui connaissent déjà HTML pour ajouter le traitement serveur à leurs pages. C’est un bon choix pour les étudiants, les amateurs, les gens en général qui sont nouveaux à la programmation. Il peut également être un bon choix pour les développeurs qui ont de l’expérience avec non-ASP.NET les technologies web.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Ai-je besoin de WebMatrix pour travailler avec Les pages Web ?

Non. WebMatrix n’est plus recommandé comme environnement de développement intégré pour ASP.NET pages Web. Utilisez [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) ou Visual Studio [Code](https://code.visualstudio.com/).

Si vous ne souhaitez pas utiliser visual Studio ou Visual Studio Code, vous pouvez installer les composants individuellement à l’aide de [Microsoft Web Platform Install](https://www.microsoft.com/web/downloads/platform.aspx). Vous avez besoin des produits suivants :

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (qui installe également le cadre ASP.NET Pages Web)
- IIS Express (le serveur web)
- Microsoft SQL Server Compact 4.0 (la base de données)

Vous pouvez utiliser un éditeur de texte pour modifier des pages *.cshtml* (ou *.vbhtml).*

Gérer les bases de données SQL Server Compact (fichiers *.sdf)* sans outil est un peu plus difficile. Visual Studio contient des outils pour gérer les bases de données *.sdf.* Vous pouvez également exécuter des commandes SQL en code pour effectuer de nombreuses tâches de gestion sqL Server.

Pour tester les pages *.cshtml* sans utiliser un environnement de développement intégré (IDE), vous pouvez les déployer sur un serveur en direct. (Voir [Puis-je déployer un site de pages Web ASP.NET sans utiliser WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Exécution DE l’IIS Express sans utiliser un IDE

Si vous installez IIS Express sur votre ordinateur comme serveur web, vous pouvez l’utiliser pour tester les pages. Vous pouvez exécuter l’IIS Express à partir de la ligne de commande et l’associer à un numéro de port spécifique. Vous spécifiez ensuite ce port lorsque vous demandez des fichiers *.cshtml* dans votre navigateur.

Dans Windows, ouvrez une invite de commande avec les privilèges de l’administrateur et changez-vous pour *les fichiers C:'Programme Fichiers’IIS Express.* (Pour les systèmes 64 bits, utilisez le dossier *C : 'Program Files (x86)'IIS Express.)* Ensuite, entrez la commande suivante, en utilisant le chemin réel vers votre site:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Vous pouvez utiliser n’importe quel numéro de port qui n’est pas déjà réservé par un autre processus. (Les numéros de port au-dessus de 1024 sont généralement gratuits.) Pour `path` la valeur, utilisez le chemin du dossier du site Web où se trouvent les fichiers *.cshtml.*

Après avoir exécuté cette commande pour configurer IIS Express pour servir vos pages, vous pouvez ouvrir un navigateur et naviguer vers un fichier *.cshtml.* Utilisez une URL comme suit :

`http://localhost:35896/default.cshtml`

Pour obtenir de l’aide avec `iisexpress.exe /?` les options de ligne de commande IIS Express, entrez à la ligne de commande.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Puis-je utiliser ASP.NET les contrôles Web de formulaires sur une page Web Pages ?

Non. Les formulaires Web sont contrôlés comme le contrôle [CheckBox,](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) les [contrôles de validation](https://msdn.microsoft.com/library/bwd43d0x)et le contrôle [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) ne fonctionnent que dans les pages Web Forms (.aspx fichiers).*.aspx* Ces contrôles nécessitent le cadre de la page Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Puis-je déployer un site de pages Web ASP.NET sans utiliser WebMatrix ?

Oui. Vous pouvez copier manuellement des fichiers de site Web sur un serveur (généralement en utilisant FTP). Si vous effectuez une copie manuelle, vous devez également copier les fichiers qui prennent en charge SQL Server Compact (la base de données). Pour plus de détails, voir l’entrée de blog [Déployant des applications Web Pages sans outil](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Dois-je utiliser l’aide WebSecurity pour prendre en charge les connexions ?

Non. Le `SimpleMembership` fournisseur qui fait partie de ASP.NET Pages Web est une option. Les fournisseurs de sécurité qui font partie de ASP.NET (que vous pourriez être habitué à travailler avec dans les formulaires Web) sont également disponibles. Par exemple, vous pouvez utiliser des formulaires d’authentification dans ASP.NET pages Web comme vous le feriez dans les formulaires Web. Pour un exemple de la façon d’utiliser l’authentification des formulaires, consultez l’article Microsoft Support [How To Implement Forms-Based Authentication in Your ASP.NET Application en utilisant C.NET](https://support.microsoft.com/kb/301240). Pour télécharger un exemple simple, voir [ASP.NET version &amp; de "Login Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Pour plus d’informations sur la façon d’utiliser l’authentification Windows, voir le blog [à l’aide de l’authentification Windows dans ASP.NET Pages Web](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Est-ce ASP.NET Pages Web prend en charge HTML5 ?

Oui. Les pages que vous créez avec ASP.NET Pages Web *(pages .cshtml* ou *.vbhtml)* sont essentiellement des pages HTML qui contiennent également du code qui s’exécute sur le serveur, avant que la page ne soit rendue. Tant que le navigateur de l’utilisateur prend en charge HTML5, vous pouvez utiliser des éléments HTML5 dans une page *.cshtml* ou *.vbhtml.*

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Puis-je utiliser JavaScript et jQuery avec des pages Web ?

Absolument. Les pages que vous créez avec ASP.NET pages Web *(pages .cshtml* ou *.vbhtml)* ne sont que des pages HTML avec du code serveur en eux. Par conséquent, tout ce que vous pouvez faire dans une page HTML normale en utilisant JavaScript ou jQuery vous pouvez également faire dans une page *.cshtml* ou *.vbhtml.*

Le modèle **starter Site** dans WebMatrix contient un certain nombre de bibliothèques jQuery. Si vous créez un site en utilisant ce modèle, le dossier *Scripts* contient une bibliothèque de base jQuery *(jquery-1.6.2.js)* et des bibliothèques pour la validation jQuery *(jquery.validate.js*, etc.).

Voici quelques billets de blog qui illustrent les façons d’utiliser jQuery avec ASP.NET Pages Web:

- [Ajout de jQuery Goodness à ASP.NET Web Pages en utilisant WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) par Rachel Appel
- [5 min : WebMatrix , jQuery UI , json et jQuery templates](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) par Jonas Eriksson
- [WebMatrix And jQuery Forms](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) par Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[ASP.NET Web Pages (Razor) - Guide de résolution des problèmes](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix et ASP.NET forum Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) sur le site web de ASP.NET
