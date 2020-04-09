---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Pages Web (Razor) API Référence rapide (fr) Microsoft Docs
author: Rick-Anderson
description: Cette page contient une liste avec de brefs exemples des objets, propriétés et méthodes les plus couramment utilisés pour la programmation ASP.NET pages Web avec la syntaxe Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676243"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET Pages Web (Razor) API Référence rapide

 par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette page contient une liste avec de brefs exemples des objets, propriétés et méthodes les plus couramment utilisés pour la programmation ASP.NET pages Web avec la syntaxe Razor.
> 
> Des descriptions marquées de " (v2)" ont été introduites dans ASP.NET version 2 des pages Web.
> 
> Pour la documentation de référence de l’API, consultez la [documentation de référence ASP.NET Pages Web](https://go.microsoft.com/fwlink/?LinkId=208659) sur MSDN.
> 
> ## <a name="software-versions"></a>Versions logicielles
> 
> 
> - ASP.NET Pages Web (Razor) 3
>   
> 
> Ce tutoriel fonctionne également avec ASP.NET pages Web 2 et ASP.NET Pages Web 1.0 (à l’exception des fonctionnalités marquées v2).

Cette page contient des informations de référence pour les éléments suivants :

- [Classes](#Classes)
- [Données](#Data)
- [Programmes d’assistance](#Helpers)
- [Validation](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Classes

### `AppState[key], AppState[index],App`

Contient des données qui peuvent être partagées par toutes les pages de l’application. Vous pouvez utiliser `App` la propriété dynamique pour accéder aux mêmes données, comme dans l’exemple suivant :

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Convertit une valeur de chaîne en valeur Boolean (vrai/faux). Retourne faux ou la valeur spécifiée si la chaîne ne représente pas vrai/faux.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Convertit une valeur de chaîne à ce jour/temps. Rendements `DateTime.MinValue` ou valeur spécifiée si la chaîne ne représente pas une date/heure.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Convertit une valeur de chaîne en valeur décimale. Rendements 0.0 ou la valeur spécifiée si la chaîne ne représente pas une valeur décimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Convertit une valeur de chaîne en flotteur. Rendements 0.0 ou la valeur spécifiée si la chaîne ne représente pas une valeur décimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Convertit une valeur de chaîne en un intégrant. Retourne 0 ou la valeur spécifiée si la chaîne ne représente pas un intégrant.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Crée une URL compatible avec le navigateur à partir d’un chemin de fichier local, avec des pièces de chemin supplémentaires en option.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Rend la *valeur* sous forme de balisage HTML au lieu de la rendre sous forme de sortie codée HTML.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Rendements vrais si la valeur peut être convertie d’une chaîne au type spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Rendements vrais si l’objet ou la variable n’a aucune valeur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Retourne vrai si la demande est un POST. (Les demandes initiales sont généralement un GET.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Spécifie le chemin d’une page de mise en page à appliquer à cette page.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Contient des données partagées entre la page, les pages de mise en page et les pages partielles dans la demande en cours. Vous pouvez utiliser `Page` la propriété dynamique pour accéder aux mêmes données, comme dans l’exemple suivant :

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Pages de mise en page) Rend le contenu d’une page de contenu qui n’est pas dans les sections nommées.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Rend une page de contenu à l’aide du chemin spécifié et des données supplémentaires facultatives. Vous pouvez obtenir les valeurs des `PageData` paramètres supplémentaires par position (exemple 1) ou clé (exemple 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Pages de mise en page) Rend une section de contenu qui a un nom. Définir *nécessaire* à faux pour rendre une section facultative.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Obtient ou définit la valeur d’un cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Obtient les fichiers qui ont été téléchargés dans la demande actuelle.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Obtient des données qui ont été affichées sous une forme (sous forme de chaînes). `Request[key]`vérifie à `Request.Form` la `Request.QueryString` fois les collections et les collections.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Obtient les données qui ont été spécifiées dans la chaîne de requête URL. `Request[key]`vérifie à `Request.Form` la `Request.QueryString` fois les collections et les collections.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Désactive sélectivement la validation des demandes pour un élément de formulaire, une valeur de chaîne de requête, un cookie ou une valeur d’en-tête. La validation des demandes est activée par défaut et empêche les utilisateurs d’afficher la balisage ou tout autre contenu potentiellement dangereux.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Ajoute un en-tête de serveur HTTP à la réponse.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Cache la sortie de la page pendant une période déterminée. Régler en option *le glissement* pour réinitialiser le délai d’attente sur chaque accès à chaque page et *varierByParams* pour mettre en cache différentes versions de la page pour chaque chaîne de requête différente dans la demande de page.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Redirige la demande de navigateur vers un nouvel emplacement.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Définit le code d’état HTTP envoyé au navigateur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Écrit le contenu des *données* à la réponse avec un type MIME optionnel.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Écrit le contenu d’un fichier à la réponse.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Pages de mise en page) Définit une section de contenu qui a un nom.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Décode une chaîne codée HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Code une chaîne pour le rendu dans la balisage HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Retourne le chemin physique du serveur pour le chemin virtuel spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Décode le texte d’une URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Code le texte pour mettre dans une URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Obtient ou définit une valeur qui existe jusqu’à ce que l’utilisateur ferme le navigateur.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Affiche une représentation de chaîne de la valeur de l’objet.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Obtient des données supplémentaires à partir de l’URL (par exemple, */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Définit le mot de passe de l'utilisateur spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Confirme un compte à l’aide du jeton de confirmation du compte.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Crée un nouveau compte utilisateur avec le nom d’utilisateur et le mot de passe spécifiés. Pour exiger un jeton de confirmation, passez vrai pour *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Obtient l’identifiant d’intégrant pour l’utilisateur actuellement connecté.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Obtient le nom de l’utilisateur actuellement connecté.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Génère un jeton de réinitialisation de mot de passe qui peut être envoyé par e-mail à un utilisateur afin que l’utilisateur puisse réinitialiser le mot de passe.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Renvoie l’identifiant de l’utilisateur à partir du nom d’utilisateur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Retourne vrai si l’utilisateur actuel est connecté.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Retourne vrai si l’utilisateur a été confirmé (par exemple, par le biais d’un e-mail de confirmation).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Retourne vrai si le nom de l’utilisateur actuel correspond au nom d’utilisateur spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Connecte l’utilisateur en définissant un jeton d’authentification dans le cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Connecte l’utilisateur en supprimant le cookie jeton d’authentification.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Si l'utilisateur n'est pas authentifié, définit l'état HTTP sur 401 (non autorisé).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Si l’utilisateur actuel n’est pas membre de l’un des rôles spécifiés, définit le statut HTTP à 401 (non autorisé).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Si l’utilisateur actuel n’est pas l’utilisateur spécifié par *nom d’utilisateur,* définit le statut HTTP à 401 (Non autorisé).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Si le jeton de réinitialisation du mot de passe est valide, modifie le mot de passe de l’utilisateur pour le nouveau mot de passe.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Données

### `Database.Execute(SQLstatement [,parameters]`

Exécute *SQLstatement* (avec paramètres facultatifs) tels que INSERT, DELETE ou UPDATE et renvoie un nombre d’enregistrements affectés.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Retourne la colonne d’identité de la rangée la plus récemment insérée.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Ouvre soit le fichier de base de données spécifié ou la base de données spécifiée à l’aide d’une chaîne de connexion nommée à partir du fichier *Web.config.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Ouvre une base de données à l’aide de la chaîne de connexion. (Cela contraste `Database.Open`avec , qui utilise un nom de chaîne de connexion.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Interroge la base de données à l’aide *de SQLstatement* (paramètres de passage optionnelle) et renvoie les résultats comme une collection.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Exécute *SQLstatement* (avec paramètres facultatifs) et renvoie un seul enregistrement.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Exécute *SQLstatement* (avec paramètres facultatifs) et renvoie une seule valeur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Programmes d’assistance

### `Analytics.GetGoogleHtml(webPropertyId)`

Rend le code JavaScript Google Analytics pour l’ID spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Rend le code JavaScript StatCounter Analytics pour le projet spécifié.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Rend le code JavaScript Yahoo Analytics pour le compte spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Passe une recherche à Bing. Pour spécifier le site à rechercher et un `Bing.SiteUrl` `Bing.SiteTitle` titre pour la boîte de recherche, vous pouvez définir le et les propriétés. Normalement, vous définissez * \_* ces propriétés dans la page AppStart.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Initialise un tableau.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Ajoute une légende à un graphique.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Ajoute une série de valeurs au graphique.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Renvoie un hachage pour les données spécifiées. L’algorithme `sha256`par défaut est .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Permet aux utilisateurs de Facebook de faire une connexion aux pages.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Rend l’interface utilisateur pour le téléchargement de fichiers.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Rend l’étiquette de jeu Xbox spécifiée.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Rend l’image Gravatar pour l’adresse e-mail spécifiée.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Convertit un objet de données en une chaîne dans le format JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Convertit une chaîne d’entrée codée par JSON en un objet de données que vous pouvez itérer ou insérer dans une base de données.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Rend les liens de réseautage social à l’aide du titre spécifié et de l’URL facultative.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Associe un message d’erreur à un champ de formulaire. Utilisez `ModelState` l’aide pour accéder à ce membre.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Associe un message d’erreur à un formulaire. Utilisez `ModelState` l’aide pour accéder à ce membre.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Retourne vrai s’il n’y a pas d’erreurs de validation. Utilisez `ModelState` l’aide pour accéder à ce membre.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Rend les propriétés et les valeurs d’un objet et de tout objet pour enfants.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Rend le test de vérification reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Définit les clés publiques et privées pour le service reCAPTCHA. Normalement, vous définissez * \_* ces propriétés dans la page AppStart.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Retourne le résultat du test reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Rend les informations sur l’état des pages Web ASP.NET.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Rend un flux Twitter pour l’utilisateur spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Rend un flux Twitter pour le texte de recherche spécifié.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Rend un lecteur vidéo Flash pour le fichier spécifié avec la largeur et la hauteur facultatives.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Rend un lecteur Windows Media pour le fichier spécifié avec la largeur et la hauteur facultatives.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Rend un lecteur Silverlight pour le fichier *.xap* spécifié avec la largeur et la hauteur requises.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Retourne l’objet spécifié par *clé,* ou nul si l’objet n’est pas trouvé.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Supprime l’objet spécifié par *la clé* du cache.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Met la *valeur* dans le cache sous le nom spécifié par *la clé*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Crée un `WebGrid` nouvel objet à l’aide de données provenant d’une requête.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Rend la balisage pour afficher les données dans une table HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Rend un téléavertisseur pour l’objet. `WebGrid`

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Charge une image à partir du chemin spécifié.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Ajoute l’image spécifiée comme filigrane.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Ajoute le texte spécifié à l’image.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Retourne l’image horizontalement ou verticalement.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Charge une image lorsqu’une image est affichée sur une page lors d’un téléchargement de fichier.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Resizes une image.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Tourne l’image vers la gauche ou vers la droite.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Enregistre l’image sur le chemin spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Définit le mot de passe pour le serveur SMTP. Normalement, vous définissez * \_* cette propriété dans la page AppStart.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Envoie un message électronique.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Définit le nom du serveur SMTP. Normalement, vous définissez * \_* cette propriété dans la page AppStart.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Définit le nom d’utilisateur du serveur SMTP. Normalement, vous devez définir * \_* cette propriété dans la page AppStart.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Validation

### `Html.ValidationMessage(field)`

(v2) Rend un message d’erreur de validation pour le champ spécifié.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Affiche une liste de toutes les erreurs de validation.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Enregistre un élément d’entrée utilisateur pour le type de validation spécifié.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Rend dynamiquement les attributs de la classe CSS pour la validation côté client afin que vous puissiez formater les messages d’erreur de validation. (Exige que vous référez les bibliothèques de script client appropriées et que vous définissiez les classes CSS.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Permet la validation côté client pour le champ d’entrée de l’utilisateur. (Exige que vous référez les bibliothèques de script client appropriées.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Les rendements sont vrais si tous les éléments d’entrée de l’utilisateur qui sont enregistrés pour validation contiennent des valeurs valides.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Précise que les utilisateurs doivent fournir une valeur pour l’élément d’entrée de l’utilisateur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Précise que les utilisateurs doivent fournir des valeurs pour chacun des éléments d’entrée de l’utilisateur. Cette méthode ne vous permet pas de spécifier un message d’erreur personnalisé.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2) Spécifie un test `Validation.Add` de validation lorsque vous utilisez la méthode.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
