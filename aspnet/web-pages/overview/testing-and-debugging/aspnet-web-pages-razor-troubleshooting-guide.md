---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) Troubleshooting Guide | Microsoft Docs
author: Rick-Anderson
description: Cet article décrit les problèmes que vous pourriez rencontrer lorsque vous travaillez avec ASP.NET Web Pages (Razor) et certaines solutions suggérées. Versions logicielles ASP.NET Web Pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: adbaa5cbda4a60a8b222ba49bb148b28b2e214cc
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59389204"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Pages (Razor) - Guide de résolution des problèmes

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit les problèmes que vous pourriez rencontrer lorsque vous travaillez avec ASP.NET Web Pages (Razor) et certaines solutions suggérées.
> 
> ## <a name="software-versions"></a>Versions des logiciels
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2 et ASP.NET Web Pages 1.0.


Cette rubrique contient les sections suivantes :

- [Problèmes avec les Pages en cours d’exécution](#Issues_Running_.cshtml_Pages)
- [Problèmes avec le Code Razor](#IssuesWithRazorCode)
- [Problèmes de sécurité et d’appartenance](#membership)
- [Problèmes liés à l’envoi de courrier électronique](#email)
- [Ressources supplémentaires](#AdditionalResources)

Pour les questions générales, consultez [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problèmes avec les Pages en cours d’exécution

De nombreux problèmes peut empêcher *.cshtml* et *.vbhtml* pages de s’exécuter correctement. Cette section répertorie les messages d’erreur courants et les causes probables.

### <a name="http-error-403---forbidden-access-is-denied"></a>Erreur HTTP 403 - Interdit : Accès refusé

*Vous n’êtes pas autorisé à afficher ce répertoire ou cette page en utilisant les informations d’identification que vous avez fournies.*

Cette erreur peut se produire si le serveur n’exécute pas la version correcte du .NET Framework. Assurez-vous que l’ordinateur qui exécute le serveur (localement ou à distance) dispose au moins .NET Framework 4 est installé. Assurez-vous également que l’application elle-même est configurée pour exécuter la bonne version.

Si ce problème localement tout en travaillant dans WebMatrix, cliquez sur le **Site** espace de travail et, dans l’arborescence, cliquez sur **paramètres**. Dans le **sélectionnez .NET Framework Version** , sélectionnez **.NET 4 (intégré)**. Si cette version est déjà définie, essayez d’exécuter WebMatrix en tant qu’administrateur.

Assurez-vous que la racine de votre site Web dispose d’au moins un *.cshtml* fichier qu’il contient.

Si vous voyez cette erreur lorsque le serveur web se trouve sur un serveur distant, contactez l’administrateur du serveur. Assurez-vous que le serveur possède le .NET Framework 4 ou version ultérieure, installé. Assurez-vous également que l’application s’exécute dans un pool d’applications est configuré pour utiliser cette version du.NET Framework.

Si vous avez un contrôle sur le serveur, assurez-vous qu’il exécute la version correcte du .NET Framework. Vous pouvez également essayer de réparer l’installation en exécutant le `aspnet_regiis -iru` commande. (Par exemple, si vous installez IIS après avoir installé le .NET Framework, IIS ne sera pas être correctement configurée pour exécuter des pages ASP.NET.) Pour plus d’informations, consultez [outil ASP.NET IIS Registration (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Erreur HTTP 403.14 - interdit

*Le serveur Web est configuré pour ne pas afficher le contenu de ce répertoire.*

Cette erreur peut se produire si vous demandez une ressource protégée (comme le *Web.config* fichier) ou qui se trouve dans un dossier protégé (comme *application\_données* ou *application\_Code*).

### <a name="http-error-40417---not-found"></a>Erreur HTTP 404.17 - introuvable

*Le contenu demandé semble être le script et n’est pas traitée par le Gestionnaire de fichiers statiques.*

Cette erreur peut se produire si le serveur n’est pas configuré correctement pour utiliser le .NET Framework 4 ou version ultérieure et par conséquent ne reconnaît pas le code dans `@{ }` blocs. Consultez la description de précédemment *HTTP erreur 403 - Interdit : L’accès est refusé*.

### <a name="http-error-4047---not-found"></a>Erreur HTTP 404.7 - non trouvé

*Le module de filtrage des demandes sont configuré pour refuser l’extension de fichier*

Cette erreur peut se produire si *.cshtml* ou *.vbhtml* extensions ont été explicitement bloquées sur le serveur. Un symptôme de ce problème est que le travail URL lorsqu’ils n’incluent pas l’extension, mais les URL qui incluent *.cshtml* ou *.vbhtml* ne fonctionnent pas. Une solution possible consiste à réactiver les extensions dans le site *Web.config* fichier. L’exemple suivant montre comment activer le *.cshtml* extension.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Erreur HTTP 404.8 - non trouvé

*Le module de filtrage des demandes sont configuré pour refuser un chemin d’accès dans l’URL qui contient une section hiddenSegment.*

Cette erreur peut se produire si vous demandez une ressource protégée (comme le *Web.config* fichier) ou qui se trouve dans un dossier protégé (comme *application\_données* ou *application\_Code*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Ce type de page non pris en charge (erreur de serveur dans l’Application '/')

Consultez la description précédemment pour 404.17 d’erreur HTTP.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problèmes avec le code Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Le nom '*classe*' n’existe pas dans le contexte actuel

Souvent, une raison que vous voyez cette erreur est que `class` références d’une assistance, mais l’application d’assistance n’est pas installé. Par exemple, si vous essayez d’utiliser un programme d’assistance, mais si vous n’avez pas installé le package à partir de NuGet, vous verrez cette erreur. Utiliser la galerie dans WebMatrix pour rechercher et installer l’application d’assistance.

Si l’application d’assistance est installée, mais la page toujours ne reconnaît pas, essayez d’ajouter ajouter un `using` instruction au code. Dans la `using` instruction, référence de l’espace de noms qui inclut l’application d’assistance. Par exemple, les programmes d’assistance base qui se trouvent dans le package de programmes d’assistance ASP.NET Web sont dans le `System.Web.Helpers` espace de noms. En haut de la page où vous souhaitez utiliser l’application d’assistance, ajoutez cette ligne :

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problèmes de sécurité et d’appartenance

Si vous utilisez le système de sécurité intégrés (appartenance) dans ASP.NET Web Pages (Razor), vous pouvez rencontrer les problèmes suivants.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Pour appeler cette méthode, la propriété « Membership.Provider » doit être une instance de « ExtendedMembershipProvider »

Cette erreur peut indiquer qu’aucun `AspNetSqlMembershipProvider` classe est configurée. (Un symptôme est que le site fonctionne correctement localement, mais génère cette erreur lorsque vous publiez celui-ci au serveur d’un fournisseur d’hébergement.) Un correctif pour résoudre ce problème consiste à activer explicitement l’appartenance simple en ajoutant le code suivant sur le site *Web.config* fichier :

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problèmes liés à l’envoi de courrier électronique

Problèmes liés à l’envoi de courrier électronique peuvent être difficile à déboguer. Un problème initial peut être que vous ne pouvez pas vous connecter au serveur SMTP. Si la connexion est établie, ASP.NET transmet le message au serveur SMTP. Toutefois, il peut y avoir des problèmes avec le message lui-même qui empêche le serveur SMTP à partir de l’envoyer.

Si votre application n’envoie pas correctement le courrier électronique, essayez ce qui suit :

- Le nom du serveur SMTP est souvent quelque chose comme `smtp.provider.com` ou `smtp.provider.net`. Toutefois, si vous publiez votre site sur un fournisseur d’hébergement, le nom du serveur SMTP à ce stade peut être `localhost`. Cette situation se produit, car une fois que vous avez publié et que votre site est en cours d’exécution sur le serveur du fournisseur, le serveur SMTP peut être local du point de vue de votre application. Cette modification dans les noms de serveur peut signifier que vous devez modifier le nom du serveur SMTP dans le cadre de votre processus de publication.
- Le numéro de port est généralement 25. Toutefois, certains fournisseurs vous obligent à utiliser port 587 ou un autre port. Contactez le propriétaire du serveur SMTP quel numéro de port qu’elles attendent vous permettent d’utiliser.
- Assurez-vous que vous utilisez les informations d’identification correctes. Si vous avez publié votre site vers un fournisseur d’hébergement, utilisez les informations d’identification que le fournisseur a spécifiquement sont pour le courrier électronique. Ces informations d’identification peuvent différer les informations d’identification que vous utilisez pour publier.
- Parfois, vous n’avez pas besoin d’informations d’identification du tout. Si vous envoyez un e-mail à l’aide de votre fournisseur de services Internet, votre fournisseur de messagerie déjà connaissez peut-être vos informations d’identification. Une fois que vous publiez, vous devrez peut-être utiliser différentes informations d’identification que lorsque vous testez sur votre ordinateur local.
- Si votre fournisseur de messagerie utilise le chiffrement, définissez `WebMail.EnableSsl` à `true`.

S’il existe une erreur d’envoi de courrier électronique, vous pouvez voir un message d’erreur ASP.NET standard, qui ressemble à ceci :

![Message d’erreur ASP.NET lorsqu’il existe un problème avec l’adresse e-mail](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Vous pouvez également déboguer des problèmes liés à l’envoi de courrier électronique à l’aide un `try-catch` bloc, comme dans l’exemple suivant. Lorsque vous utilisez un `try-catch` bloc, ASP.NET n’affiche pas ses messages d’erreur standard. Au lieu de cela, vous pouvez capturer l’erreur dans la `catch` partie du bloc.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Remplacez les valeurs appropriées pour `your-SMTP-server-name`, et ainsi de suite. Voici quelques-unes des messages d’erreur que peut s’afficher de cette façon :

- *Échec de l’envoi de courrier.*

    - ou -

    *Une tentative de connexion a échoué car la partie connectée n’a pas répondu convenablement après une période de temps ou la connexion établie a échoué, car l’hôte connecté n’a pas répondu*

    Cette erreur signifie généralement que l’application ne peut pas se connecter au serveur SMTP. Vérifiez le nom du serveur et le numéro de port.
- *La boîte aux lettres non disponible. La réponse du serveur était : 5.1.0 &lt; someuser@invaliddomain &gt; expéditeur rejeté : domaine de l’expéditeur non valide*

    Ce message peut indiquer que le `From` adresse n’est pas correcte ou est manquant.
- *La chaîne spécifiée n’est pas sous la forme requise pour une adresse de messagerie.*

    Cette erreur peut indiquer que la valeur de la `To` ou `From` propriétés ne sont pas reconnues comme adresses de messagerie. (ASP.NET ne peut pas vérifier que l’adresse de messagerie est valide, uniquement si elle 's dans le format correct, comme *name@domain.com*.)

> [!NOTE]
> Supprimez la balise qui affiche l’erreur (`@errorMessage`) avant de publier la page sur un site de production. Il n’est pas une bonne idée pour permettre aux utilisateurs de voir les messages d’erreur que vous obtenez à partir d’un serveur.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[ASP.NET Web Pages - Questions fréquentes (FAQ) (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix et ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum sur le site Web ASP.NET
