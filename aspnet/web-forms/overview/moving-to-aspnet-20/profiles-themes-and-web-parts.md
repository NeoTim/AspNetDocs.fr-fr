---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profils, thèmes et parties Web (en anglais seulement) Microsoft Docs
author: rick-anderson
description: Il y a des changements majeurs dans la configuration et l’instrumentation dans ASP.NET 2.0. La nouvelle configuration ASP.NET API permet de modifier la configuration...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 4bc98cca226a0bd9bd766a21e88b0facf2a4b610
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543637"
---
# <a name="profiles-themes-and-web-parts"></a>Profils, thèmes et composants WebPart

par [Microsoft](https://github.com/microsoft)

> Il y a des changements majeurs dans la configuration et l’instrumentation dans ASP.NET 2.0. La nouvelle configuration ASP.NET API permet d’apporter des modifications de configuration programmatiquement. En outre, de nombreux nouveaux paramètres de configuration existent permettent de nouvelles configurations et l’instrumentation.

ASP.NET 2.0 représente une amélioration substantielle dans le domaine des sites Web personnalisés. En plus des fonctionnalités d’adhésion que nous avons déjà couvertes, ASP.NET profils, thèmes et parties Web améliorent considérablement la personnalisation des sites Web.

## <a name="aspnet-profiles"></a>Profils ASP.NET

ASP.NET profils sont similaires aux sessions. La différence est qu’un profil est persistant alors qu’une session est perdue lorsque le navigateur est fermé. Une autre grande différence entre les sessions et les profils est que les profils sont fortement tapés, vous fournissant ainsi IntelliSense pendant le processus de développement.

Un profil est défini dans le fichier de configuration des machines ou dans le fichier web.config pour l’application. (Vous ne pouvez pas définir un profil dans un fichier web.config de sous-dossiers.) Le code ci-dessous définit un profil pour stocker les visiteurs du site Web en premier et le nom de famille.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Le type de données par défaut pour une propriété de profil est System.String. Dans l’exemple ci-dessus, aucun type de données n’a été spécifié. Par conséquent, les propriétés FirstName et LastName sont toutes deux de type String. Comme mentionné précédemment, les propriétés de profil sont fortement tapées. Le code ci-dessous ajoute une nouvelle propriété pour l’âge qui est de type Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Les profils sont généralement utilisés avec ASP.NET l’authentification des formulaires. Lorsqu’il est utilisé en combinaison avec l’authentification des formulaires, chaque utilisateur a un profil distinct associé à son identifiant d’utilisateur. Cependant, il est également possible d’autoriser l’utilisation &lt;de profils dans une application anonyme en utilisant l’élément d’identification&gt; anonyme dans le fichier de configuration avec **l’attribut autorisé anonyme** comme indiqué ci-dessous:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Lorsqu’un utilisateur anonyme navigue sur le site, ASP.NET crée une instance de **ProfileCommon** pour l’utilisateur. Ce profil utilise un IDENTIFIANT unique stocké dans un cookie sur le navigateur pour identifier l’utilisateur comme un visiteur unique. De cette façon, vous pouvez stocker des informations de profil pour les utilisateurs qui naviguent anonymement.

## <a name="profile-groups"></a>Groupes de profil

Il est possible de regrouper les propriétés des profils. En mégroupant les propriétés, il est possible de simuler plusieurs profils pour une application spécifique.

La configuration suivante configure une propriété FirstName et LastName pour deux groupes ; Acheteurs et Prospects.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Il est alors possible de définir des propriétés sur un groupe particulier comme suit :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Stockage d’objets complexes

Jusqu’à présent, les exemples que nous avons couverts ont stocké des types de données simples dans un profil. Il est également possible de stocker des types de données complexes dans un profil en spécifiant la méthode de sérialisation à l’aide de l’attribut **sérialisationAs** comme suit:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

Dans ce cas, le type est PurchaseInvoice. La classe PurchaseInvoice doit être marquée comme sérialisable et peut contenir n’importe quel nombre de propriétés. Par exemple, si PurchaseInvoice a une propriété appelée **NumItemsPurchased**, vous pouvez vous référer à cette propriété dans le code comme suit:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Héritage de profil

Il est possible de créer un profil pour une utilisation dans plusieurs applications. En créant une classe de profil qui dérive de ProfileBase, vous pouvez réutiliser un profil dans plusieurs applications en utilisant l’attribut **héritière** tel qu’indiqué ci-dessous :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

Dans ce cas, la classe **PurchasingProfile** ressemblerait à ceci :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Fournisseurs de profils

ASP.NET profils utilisent le modèle fournisseur. Le fournisseur par défaut stocke les informations dans\_une base de données SQL Server Express dans le dossier App Data de l’application Web à l’aide du fournisseur SqlProfileProvider. Si la base de données n’existe pas, ASP.NET la créera automatiquement lorsque le profil tente de stocker des informations.

Dans certains cas, cependant, vous pouvez développer votre propre fournisseur de profil. La fonction de profil ASP.NET vous permet d’utiliser facilement différents fournisseurs.

Vous créez un fournisseur de profil personnalisé lorsque :

- Vous devez stocker des informations de profil dans une source de données, comme dans une base de données FoxPro ou dans une base de données Oracle, qui n’est pas prise en charge par les fournisseurs de profils inclus dans le cadre .NET.
- Vous devez gérer les informations de profil à l’aide d’un schéma de base de données qui est différent du schéma de base de données utilisé par les fournisseurs inclus dans le cadre .NET. Un exemple courant est que vous souhaitez intégrer des informations de profil avec les données utilisateur dans une base de données SQL Server existante.

### <a name="required-classes"></a>Classes requises

Pour implémenter un fournisseur de profil, vous créez une classe qui hérite de la classe abstraite System.Web.Profile.ProfileProvider. La classe abstraite **ProfileProvider** hérite à son tour de la classe abstraite System.Configuration.SettingsProvider, qui hérite de la classe abstraite System.Configuration.Provider.ProviderBase. En raison de cette chaîne d’héritage, en plus des membres requis de la classe **ProfileProvider,** vous devez mettre en œuvre les membres requis des classes **SettingsProvider** et **ProviderBase.**

Les tableaux suivants décrivent les propriétés et les méthodes que vous devez mettre en œuvre à partir des classes abstraites **ProviderBase**, **SettingsProvider**et **ProfileProvider.**

### <a name="providerbase-members"></a>Membres ProviderBase

| **Membre** | **Description** |
| --- | --- |
| Initialize (méthode) | Prend comme entrée le nom de l’instance du fournisseur et une NameValueCollection des paramètres de configuration. Utilisé pour définir les options et les valeurs de propriété pour l’instance fournisseur, y compris les valeurs et les options spécifiques à la mise en œuvre spécifiées dans la configuration de la machine ou le fichier Web.config. |

### <a name="settingsprovider-members"></a>ParamètresProvider les membres

| **Membre** | **Description** |
| --- | --- |
| Propriété ApplicationName | Le nom de l’application qui est stocké avec chaque profil. Le fournisseur de profil utilise le nom de l’application pour stocker les informations de profil séparément pour chaque application. Cela permet à plusieurs applications ASP.NET d’utiliser la même source de données sans conflit si le même nom d’utilisateur est créé dans différentes applications. Alternativement, plusieurs applications ASP.NET peuvent partager une source de données de profil en spécifiant le même nom d’application. |
| Méthode GetPropertyValues | Prend comme entrée un ParamètresContext et un objet SettingsPropertyCollection. Le **SettingsContext** fournit des informations sur l’utilisateur. Vous pouvez utiliser ces informations comme clé principale pour récupérer les informations de propriété du profil pour l’utilisateur. Utilisez l’objet **SettingsContext** pour obtenir le nom d’utilisateur et si l’utilisateur est authentifié ou anonyme. Le **SettingsPropertyCollection** contient une collection d’objets SettingsProperty. Chaque objet **ParamètresProperty** fournit le nom et le type de la propriété ainsi que des informations supplémentaires telles que la valeur par défaut pour la propriété et si la propriété est lue uniquement. La méthode **GetPropertyValues** remplit un SettingsPropertyValueCollection avec des objets SettingsPropertyValue basés sur les objets **SettingsProperty** fournis comme entrée. Les valeurs de la source de données de l’utilisateur spécifié sont attribuées aux propriétés PropertyValue pour chaque objet **SettingsPropertyValue** et la collection entière est retournée. L’appel de la méthode met également à jour la valeur LastActivDate pour le profil utilisateur spécifié à la date et à l’heure actuelles. |
| Méthode SetPropertyValues | Prend comme entrée un **ParamètresContext** et un objet **SettingsPropertyValueCollection.** Le **SettingsContext** fournit des informations sur l’utilisateur. Vous pouvez utiliser ces informations comme clé principale pour récupérer les informations de propriété du profil pour l’utilisateur. Utilisez l’objet **SettingsContext** pour obtenir le nom d’utilisateur et si l’utilisateur est authentifié ou anonyme. Les **ParamètresPropertyValueCollection** contient une collection d’objets **SettingsPropertyValue.** Chaque objet **ParamètresPropertyValue** fournit le nom, le type et la valeur de la propriété ainsi que des informations supplémentaires telles que la valeur par défaut pour la propriété et si la propriété est lue uniquement. La méthode **SetPropertyValues** met à jour la valeur de la propriété de profil dans la source de données pour l’utilisateur spécifié. L’appel de la méthode met également à jour les valeurs **LastActivDate** et LastUpdatedDate pour le profil utilisateur spécifié à la date et à l’heure actuelles. |

### <a name="profileprovider-members"></a>Membres ProfileProvider

| **Membre** | **Description** |
| --- | --- |
| Méthode DeleteProfiles | Prend comme entrée un éventail de noms d’utilisateur et supprime de la source de données toutes les informations de profil et les valeurs de propriété pour les noms spécifiés où le nom de l’application correspond à la valeur de la propriété **ApplicationName.** Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et que vous annulez la transaction et que vous jetez une exception en cas d’échec de l’opération de suppression. |
| Méthode DeleteProfiles | Prend comme entrée une collection d’objets ProfileInfo et supprime à partir de la source de données toutes les informations de profil et les valeurs de propriété pour chaque profil où le nom de l’application correspond à la valeur de la propriété **ApplicationName.** Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et de annuler la transaction et de lancer une exception en cas d’échec de toute opération de suppression. |
| Supprimer la méthode Desprofiles actives | Prend comme entrée une valeur ProfileAuthenticationOption et un objet DateTime et supprime à partir de la source de données toutes les informations de profil et les valeurs de propriété lorsque la dernière date d’activité est inférieure ou égale à la date et l’heure spécifiées et où le nom de l’application correspond à la valeur de la propriété **ApplicationName.** Le **paramètre ProfileAuthenticationOption** précise si seuls les profils anonymes, seuls les profils authentifiés ou tous les profils doivent être supprimés. Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et de annuler la transaction et de lancer une exception en cas d’échec de toute opération de suppression. |
| Méthode GetAllProfiles | Prend comme entrée une valeur **ProfileAuthenticationOption,** un intégrant qui spécifie l’index de la page, un intégrer qui spécifie la taille de la page, et une référence à un intégrer qui sera réglé sur le nombre total de profils. Renvoie un ProfileInfoCollection qui contient des objets **ProfileInfo** pour tous les profils de la source de données où le nom de l’application correspond à la valeur de la propriété **ApplicationName.** Le **paramètre ProfileAuthenticationOption** précise si seuls des profils anonymes, uniquement des profils authentifiés ou tous les profils doivent être retournés. Les résultats retournés par la méthode **GetAllProfiles** sont limités par l’index de page et les valeurs de taille des pages. La valeur de la taille de la page spécifie le nombre maximum d’objets **ProfileInfo** à retourner dans le **ProfileInfoCollection**. La valeur de l’index de la page précise quelle page de résultats à retourner, où 1 identifie la première page. Le paramètre pour les enregistrements totaux est un paramètre (vous pouvez utiliser **ByRef** dans Visual Basic) qui est réglé sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et que la valeur de l’indice de page est de 2 avec une taille de page de 5, le **ProfileInfoCollection** retourné contient le sixième à travers les dixième profils. La valeur totale des enregistrements est fixée à 13 lorsque la méthode revient. |
| Méthode GetAllInactiveProfiles | Prend comme entrée une valeur **ProfileAuthenticationOption,** un objet **DateTime,** un intégrer qui spécifie l’index de la page, un intégrer qui spécifie la taille de la page, et une référence à un intégrer qui sera réglé sur le nombre total de profils. Renvoie un **ProfileInfoCollection** qui contient des objets **ProfileInfo** pour tous les profils de la source de données où la dernière date d’activité est inférieure ou égale à la **DateTime** spécifiée et où le nom de l’application correspond à la valeur de la propriété **ApplicationName.** Le **paramètre ProfileAuthenticationOption** précise si seuls des profils anonymes, uniquement des profils authentifiés ou tous les profils doivent être retournés. Les résultats retournés par la méthode **GetAllInactiveProfiles** sont limités par l’index de page et les valeurs de taille de page. La valeur de la taille de la page spécifie le nombre maximum d’objets **ProfileInfo** à retourner dans le **ProfileInfoCollection**. La valeur de l’index de la page précise quelle page de résultats à retourner, où 1 identifie la première page. Le paramètre pour les enregistrements totaux est un paramètre (vous pouvez utiliser **ByRef** dans Visual Basic) qui est réglé sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et que la valeur de l’indice de page est de 2 avec une taille de page de 5, le **ProfileInfoCollection** retourné contient le sixième à travers les dixième profils. La valeur totale des enregistrements est fixée à 13 lorsque la méthode revient. |
| Trouver La méthode FindProfilesByUserName | Prend comme entrée une valeur **ProfileAuthenticationOption,** une chaîne contenant un nom d’utilisateur, un intégrant qui spécifie l’index de la page, un intégrer qui spécifie la taille de la page, et une référence à un intégrant qui sera réglé sur le nombre total de profils. Renvoie un **ProfileInfoCollection** qui contient des objets **ProfileInfo** pour tous les profils de la source de données où le nom d’utilisateur correspond au nom d’utilisateur spécifié et où le nom de l’application correspond à la valeur de la propriété **ApplicationName.** Le **paramètre ProfileAuthenticationOption** précise si seuls des profils anonymes, uniquement des profils authentifiés ou tous les profils doivent être retournés. Si votre source de données prend en charge des fonctionnalités de recherche supplémentaires, telles que des caractères wildcard, vous pouvez fournir des capacités de recherche plus étendues pour les noms d’utilisateurs. Les résultats retournés par la méthode **FindProfilesByUserName** sont limités par l’index de page et les valeurs de taille des pages. La valeur de la taille de la page spécifie le nombre maximum d’objets **ProfileInfo** à retourner dans le **ProfileInfoCollection**. La valeur de l’index de la page précise quelle page de résultats à retourner, où 1 identifie la première page. Le paramètre pour les enregistrements totaux est un paramètre (vous pouvez utiliser **ByRef** dans Visual Basic) qui est réglé sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et que la valeur de l’indice de page est de 2 avec une taille de page de 5, le **ProfileInfoCollection** retourné contient le sixième à travers les dixième profils. La valeur totale des enregistrements est fixée à 13 lorsque la méthode revient. |
| TrouverInactiveProfilesByUserName méthode | Prend comme entrée une valeur **ProfileAuthenticationOption,** une chaîne contenant un nom d’utilisateur, un objet **DateTime,** un intégrer qui spécifie l’index de la page, un intégrer qui spécifie la taille de la page, et une référence à un intégrer qui sera réglé sur le nombre total de profils. Renvoie un **ProfileInfoCollection** qui contient des objets **ProfileInfo** pour tous les profils de la source de données où le nom d’utilisateur correspond au nom d’utilisateur spécifié, où la dernière date d’activité est inférieure ou égale à la **DateTime**spécifiée, et où le nom de l’application correspond à la valeur de la propriété **ApplicationName.** Le **paramètre ProfileAuthenticationOption** précise si seuls des profils anonymes, uniquement des profils authentifiés ou tous les profils doivent être retournés. Si votre source de données prend en charge des fonctionnalités de recherche supplémentaires, telles que des caractères wildcard, vous pouvez fournir des capacités de recherche plus étendues pour les noms d’utilisateurs. Les résultats retournés par la méthode **FindInactiveProfilesByUserName** sont limités par l’index de page et les valeurs de taille des pages. La valeur de la taille de la page spécifie le nombre maximum d’objets **ProfileInfo** à retourner dans le **ProfileInfoCollection**. La valeur de l’index de la page précise quelle page de résultats à retourner, où 1 identifie la première page. Le paramètre pour les enregistrements totaux est un paramètre (vous pouvez utiliser **ByRef** dans Visual Basic) qui est réglé sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et que la valeur de l’indice de page est de 2 avec une taille de page de 5, le **ProfileInfoCollection** retourné contient le sixième à travers les dixième profils. La valeur totale des enregistrements est fixée à 13 lorsque la méthode revient. |
| Méthode GetNumberOfInActiveProfiles | Prend comme entrée une valeur **ProfileAuthenticationOption** et un objet **DateTime** et renvoie un nombre de tous les profils dans la source de données où la dernière date d’activité est inférieure ou égale à la **DateTime** spécifiée et où le nom de l’application correspond à la valeur de la propriété **ApplicationName.** Le **paramètre ProfileAuthenticationOption** précise si seuls des profils anonymes, uniquement des profils authentifiés ou tous les profils doivent être comptés. |

### <a name="applicationname"></a>ApplicationName

Étant donné que les fournisseurs de profil stockent les informations de profil séparément pour chaque application, vous devez vous assurer que votre schéma de données inclut le nom de l’application et que les requêtes et mises à jour incluent également le nom de l’application. Par exemple, la commande suivante est utilisée pour récupérer une valeur de propriété à partir d’une base de données basée sur le nom d’utilisateur et si le profil est anonyme, et s’assure que la valeur **ApplicationName** est incluse dans la requête.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>thèmes ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Quels sont ASP.NET thèmes 2.0 ?

L’un des aspects les plus importants d’une application Web est un regard et une sensation cohérents sur l’ensemble du site. ASP.NET développeurs 1.x utilisent habituellement Des feuilles de style en cascade (CSS) pour mettre en œuvre un look et une sensation cohérents. ASP.NET thèmes 2.0 améliorent considérablement sur CSS parce qu’ils donnent au développeur ASP.NET la possibilité de définir l’apparence des contrôles de serveur ASP.NET ainsi que des éléments HTML. ASP.NET thèmes peuvent être appliqués aux contrôles individuels, à une page Web spécifique ou à toute une application Web. Les thèmes utilisent une combinaison de fichiers CSS, un fichier skin optionnel et un répertoire Images optionnel si des images sont nécessaires. Le fichier de la peau contrôle l’apparence visuelle des commandes ASP.NET serveur.

## <a name="where-are-themes-stored"></a>Où sont stockés les thèmes ?

L’emplacement où les thèmes sont stockés diffère en fonction de leur portée. Les thèmes qui peuvent être appliqués à n’importe quelle application sont stockés dans le dossier suivant :

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un thème spécifique à une application particulière `App\_Themes\<Theme\_Name>` est stocké dans un répertoire à la racine du site Web.

> [!NOTE]
> Un fichier de peau ne doit modifier que les propriétés de contrôle du serveur qui affectent l’apparence.

Un thème global est un thème qui peut être appliqué à n’importe quelle application ou site Web en cours d’exécution sur le serveur Web. Ces thèmes sont stockés par défaut dans le répertoire ASP.NETClientfiles-Themes qui se trouve à l’intérieur de l’annuaire v2.x.xxxxx. Alternativement, vous pouvez déplacer les fichiers\_thématiques\_dans le site Web/[version] du client/système aspnet/Thèmes/[nom du thème]\_dans la racine de votre site Web.

Les thèmes spécifiques à l’application ne peuvent être appliqués qu’à l’application dans laquelle résident les fichiers. Ces fichiers sont `App\_Themes/<theme\_name>` stockés dans l’annuaire à la racine du site Web.

## <a name="the-components-of-a-theme"></a>Les composantes d’un thème

Un thème est composé d’un ou plusieurs fichiers CSS, d’un fichier skin optionnel et d’un dossier Images optionnel. Les fichiers CSS peuvent être n’importe quel nom que vous souhaitez (c.-à-d. default.css ou theme.css, etc.) et doivent être à la racine du dossier de thèmes. Les fichiers CSS sont utilisés pour définir les classes et les attributs CSS ordinaires pour des sélecteurs spécifiques. Pour appliquer l’une des classes CSS à un élément de page, la propriété **CSSClass** est utilisée.

Le fichier de peau est un fichier XML qui contient des définitions de propriété pour ASP.NET les contrôles du serveur. Le code énuméré ci-dessous est un fichier de peau d’exemple.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Figure 1** ci-dessous montre une petite page ASP.NET parcourue sans un thème appliqué. **La figure 2** affiche le même fichier avec un thème appliqué. La couleur de fond et la couleur du texte sont configurées via un fichier CSS. L’apparence du bouton et de la boîte texte est configurée à l’aide du fichier de la peau indiqué ci-dessus.

![Aucun thème](profiles-themes-and-web-parts/_static/image1.gif)

**Figure 1**: Pas de thème

![Thème appliqué](profiles-themes-and-web-parts/_static/image2.gif)

**Figure 2**: Thème appliqué

Le fichier de la peau énuméré ci-dessus définit une peau par défaut pour tous les contrôles TextBox et les contrôles bouton. Cela signifie que chaque contrôle TextBox et contrôle bouton inséré sur une page prendra sur cette apparence. Vous pouvez également définir une peau qui peut être appliquée à des instances spécifiques de ces contrôles en utilisant la propriété **SkinID** du contrôle.

Le code ci-dessous définit une peau pour un contrôle de bouton. Seuls les commandes de bouton avec une propriété **SkinID** de **goButton** prendra l’apparence de la peau.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Vous ne pouvez avoir qu’une peau par défaut par type de contrôle serveur. Si vous avez besoin de peaux supplémentaires, vous devriez utiliser la propriété SkinID.

## <a name="applying-themes-to-pages"></a>Appliquer des thèmes aux pages

Un thème peut être appliqué à l’aide de l’une des méthodes suivantes :

- Dans &lt;l’élément pages&gt; du fichier web.config
- Dans @Page la directive d’une page
- Par programmation

## <a name="applying-a-theme-in-the-configuration-file"></a>Application d’un thème dans le fichier Configuration

Pour appliquer un thème dans le fichier de configuration des applications, utilisez la syntaxe suivante :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Le nom du thème spécifié ici doit correspondre au nom du dossier thèmes. Ce dossier peut exister soit dans l’un des endroits mentionnés plus tôt dans ce cours. Si vous essayez d’appliquer un thème qui n’existe pas, une erreur de configuration se produira.

## <a name="applying-a-theme-in-the-page-directive"></a>Appliquer un thème dans la directive Page

Vous pouvez également appliquer un thème dans la directive Page. Cette méthode vous permet d’utiliser un thème pour une page spécifique.

Pour appliquer un @Page thème dans la directive, utilisez la syntaxe suivante :

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Une fois de plus, le thème spécifié ici doit correspondre au dossier de thème comme mentionné précédemment. Si vous essayez d’appliquer un thème qui n’existe pas, une défaillance de construction se produira. Visual Studio mettra également en évidence l’attribut et vous informera qu’aucun tel thème n’existe.

## <a name="applying-a-theme-programmatically"></a>Appliquer un thème programmatiquement

Pour appliquer un thème programmatiquement, vous devez spécifier la propriété **Thème** pour la page dans la méthode **PréInit\_De page.**

Pour appliquer un thème programmatiquement, utilisez la syntaxe suivante :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Il est nécessaire d’appliquer le thème dans la méthode PreInit en raison du cycle de vie de la page. Si vous l’appliquez après ce point, le thème des pages aura déjà été appliqué par le temps d’exécution et un changement à ce stade est trop tard dans le cycle de vie. Si vous appliquez un thème qui n’existe pas, une **impression httpexception** se produit. Lorsqu’un thème est appliqué de manière programmatique, un avertissement de construction se produit si des commandes de serveur ont une propriété SkinID spécifiée. Cet avertissement est destiné à vous informer qu’aucun thème n’est appliqué de façon déclarative et qu’il peut être ignoré.

## <a name="exercise-1--applying-a-theme"></a>Exercice 1 : Application d’un thème

Dans cet exercice, vous appliquerez un thème ASP.NET à un site Web.

> [!IMPORTANT]
> Si vous utilisez Microsoft Word pour entrer des informations dans un fichier skin, assurez-vous que vous ne remplacez pas les devis réguliers par des devis intelligents. Les devis intelligents causeront des problèmes avec les fichiers cutanés.

1. Créez un site web ASP.NET.
2. Cliquez à droite sur le projet dans Solution Explorer et choisissez Ajouter un nouvel article.
3. Choisissez le fichier de configuration Web dans la liste des fichiers et cliquez sur Ajouter.
4. Cliquez à droite sur le projet dans Solution Explorer et choisissez Ajouter un nouvel article.
5. Choisissez Skin File et cliquez sur Ajouter.
6. Cliquez oui lorsqu’on vous demande si vous souhaitez\_placer le fichier à l’intérieur du dossier Thèmes App.
7. Cliquez à droite sur le dossier SkinFile à l’intérieur du dossier App\_Themes dans Solution Explorer et choisissez Ajouter un nouvel élément.
8. Choisissez La feuille de style dans la liste des fichiers et cliquez sur Ajouter. Vous avez maintenant tous les fichiers nécessaires pour implémenter votre nouveau thème. Cependant, Visual Studio a nommé votre dossier de thèmes SkinFile. Cliquez à droite sur ce dossier et changez le nom pour CoolTheme.
9. Ouvrez le fichier SkinFile.skin et ajoutez le code suivant à la fin du fichier : 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Enregistrer le fichier SkinFile.skin.
11. Ouvrez le StyleSheet.css.
12. Remplacer tout le texte en elle par ce qui suit: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Enregistrez le fichier StyleSheet.css.
14. Ouvrez la page Default.aspx.
15. Ajoutez un contrôle TextBox et un contrôle bouton.
16. Enregistrez la page. Maintenant, parcourez la page Default.aspx. Il doit s’afficher sous forme Web normale.
17. Ouvrez le fichier web.config.
18. Ajouter ce qui suit `<system.web>` directement sous l’étiquette d’ouverture : 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Enregistrer le fichier web.config. Maintenant, parcourez la page Default.aspx. Il doit s’afficher avec le thème appliqué.
20. S’il n’est pas déjà ouvert, ouvrez la page Default.aspx dans Visual Studio.
21. Sélectionnez le bouton.
22. Changer la propriété **SkinID** pour allerButton. Notez que Visual Studio fournit un dropdown avec des valeurs SkinID valides pour un contrôle de bouton.
23. Enregistrez la page. Maintenant, prévisualisez à nouveau la page de votre navigateur. Le bouton devrait maintenant dire "aller" et devrait être plus large en apparence.

En utilisant la propriété **SkinID,** vous pouvez facilement configurer différentes peaux pour différents cas d’un type particulier de contrôle du serveur.

## <a name="the-stylesheettheme-property"></a>La propriété StyleSheetTheme

Jusqu’à présent, nous n’avons parlé que d’appliquer des thèmes en utilisant la propriété Thème. Lors de l’utilisation de la propriété Theme, le fichier de la peau va passer outre les paramètres déclaratifs pour les contrôles du serveur. Par exemple, dans l’exercice 1, vous avez spécifié un SkinID de "goButton" pour le contrôle du bouton et qui a changé le texte du bouton pour "aller". Vous avez peut-être remarqué que la propriété Text du bouton dans le concepteur a été mis à "Button", mais le thème a dépassé cela. Le thème remplacera toujours tous les paramètres de propriété dans le concepteur.

Si vous souhaitez être en mesure de remplacer les propriétés définies dans le fichier de la peau du thème avec des propriétés spécifiées dans le concepteur, vous pouvez utiliser la propriété **StyleSheetTheme** au lieu de la propriété Theme. La propriété StyleSheetTheme est la même que la propriété Theme, sauf qu’elle ne remplace pas tous les paramètres explicites de propriété comme la propriété Thème ne.

Pour voir cela en action, ouvrez le fichier web.config `<pages>` du projet dans l’exercice 1 et changez l’élément en ce qui suit :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Maintenant, parcourez la page Default.aspx et vous verrez que le contrôle de bouton a une propriété de texte de "Button" à nouveau. C’est parce que le cadre de propriété explicite dans le concepteur est de prépondérer la propriété Text définie par le goButton SkinID.

## <a name="overriding-themes"></a>Thèmes dominants

Un thème global peut être remplacé par l’application d’un thème du même nom dans le dossier App\_Themes de l’application. Cependant, le thème n’est pas appliqué dans un véritable scénario de remplacement. Si le temps d’exécution\_rencontre des fichiers thématiques dans le dossier Thèmes App, il appliquera le thème à l’aide de ces fichiers et ignorera le thème global.

La propriété StyleSheetTheme est primordiale et peut être remplacée dans le code comme suit:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>composants WebPart

ASP.NET Web Parts est un ensemble intégré de contrôles pour créer des sites Web qui permettent aux utilisateurs finaux de modifier le contenu, l’apparence et le comportement des pages Web directement à partir d’un navigateur. Les modifications peuvent être appliquées à tous les utilisateurs du site ou à des utilisateurs individuels. Lorsque les utilisateurs modifient des pages et des contrôles, les paramètres peuvent être enregistrés pour conserver les préférences personnelles d’un utilisateur sur les futures sessions de navigateur, une fonctionnalité appelée personnalisation. Ces fonctionnalités de Web Parts permettent aux développeurs de personnaliser dynamiquement une application Web, sans intervention du développeur ou de l’administrateur.

À l’aide de l’ensemble de contrôle Web Parts, vous en tant que développeur pouvez activer les utilisateurs finaux à :

- Personnalisez le contenu de la page. Les utilisateurs peuvent ajouter de nouveaux contrôles de pièces Web à une page, les supprimer, les cacher ou les minimiser comme des fenêtres ordinaires.
- Personnalisez la mise en page. Les utilisateurs peuvent faire glisser un contrôle de pièces Web vers une zone différente sur une page, ou changer son apparence, ses propriétés et son comportement.
- Contrôles à l’exportation et à l’importation. Les utilisateurs peuvent importer ou exporter des paramètres de contrôle des pièces Web pour une utilisation dans d’autres pages ou sites, en conservant les propriétés, l’apparence et même les données dans les contrôles. Cela réduit les exigences de saisie et de configuration des données sur les utilisateurs finaux.
- Créez des connexions. Les utilisateurs peuvent établir des connexions entre les contrôles de sorte que, par exemple, un contrôle graphique puisse afficher un graphique pour les données dans un contrôle de ticker de stock. Les utilisateurs pouvaient personnaliser non seulement la connexion elle-même, mais l’apparence et les détails de la façon dont le contrôle du graphique affiche les données.
- Gérer et personnaliser les paramètres au niveau du site. Les utilisateurs autorisés peuvent configurer les paramètres au niveau du site, déterminer qui peut accéder à un site ou une page, définir un accès basé sur les rôles aux contrôles, et ainsi de suite. Par exemple, un utilisateur dans un rôle administratif peut définir un contrôle des parties Web pour être partagé par tous les utilisateurs, et empêcher les utilisateurs qui ne sont pas administrateurs de personnaliser le contrôle partagé.

Vous travaillerez généralement avec Web Parts de l’une des trois façons suivantes : créer des pages qui utilisent des contrôles de pièces Web, créer des contrôles Individuels de pièces Web ou créer des applications Web complètes et personnalisables, comme un portail.

## <a name="page-development"></a>Développement de la page

Les développeurs de pages peuvent utiliser des outils de conception visuelle tels que Microsoft Visual Studio 2005 pour créer des pages qui utilisent des parties Web. Un avantage dans l’utilisation d’un outil tel que Visual Studio est que l’ensemble de contrôle Web Parts fournit des fonctionnalités pour la création de drag-and-drop et la configuration des contrôles de parties Web dans un concepteur visuel. Par exemple, vous pouvez utiliser le concepteur pour faire glisser une zone de parties Web, ou un contrôle d’éditeur de parties Web, sur la surface de conception, puis configurer le droit de commande dans le concepteur à l’aide de l’interface utilisateur fournie par l’ensemble de contrôle Web Parts. Cela peut accélérer le développement des applications Web Parts et réduire la quantité de code que vous devez écrire.

## <a name="control-development"></a>Développement de contrôle

Vous pouvez utiliser n’importe quel contrôle ASP.NET existant comme contrôle des parties Web, y compris les contrôles standard du serveur Web, les commandes personnalisées du serveur et les contrôles des utilisateurs. Pour un contrôle programmatique maximal de votre environnement, vous pouvez également créer des contrôles Web Parts personnalisés qui dérivent de la classe WebPart. Pour le développement individuel de contrôle des parties Web, vous créez généralement un contrôle utilisateur et l’utiliserez comme un contrôle de parties Web, ou développerez un contrôle personnalisé des pièces Web.

À titre d’exemple de développement d’un contrôle personnalisé des parties Web, vous pouvez créer un contrôle pour fournir l’une ou l’autre des fonctionnalités fournies par d’autres contrôles ASP.NET serveur qui pourraient être utiles à emballer en tant que contrôle des parties Web personnalisables : calendriers, listes, informations financières, nouvelles, calculatrices, contrôles texte riches pour la mise à jour du contenu, grilles modifiables qui se connectent aux bases de données, graphiques qui mettent à jour dynamiquement leurs écrans , ou des informations météorologiques et de voyage. Si vous fournissez à un concepteur visuel votre contrôle, alors n’importe quel développeur de page utilisant Visual Studio peut simplement faire glisser votre contrôle dans une zone de parties Web et le configurer au moment de la conception sans avoir à écrire du code supplémentaire.

La personnalisation est à la base de la fonction Web Parts. Il permet aux utilisateurs de modifier - ou personnaliser - la mise en page, l’apparence et le comportement des contrôles Web Parts sur une page. Les paramètres personnalisés sont de longue durée : ils sont persistants non seulement pendant la session de navigateur en cours (comme dans l’état de vue), mais aussi dans le stockage à long terme, de sorte que les paramètres d’un utilisateur sont sauvegardés pour les futures sessions de navigateur ainsi. La personnalisation est activée par défaut pour les pages Web Parts.

Les composants structurels de l’interface utilisateur reposent sur la personnalisation et fournissent la structure de base et les services dont tous les contrôles sur les pièces Web ont besoin. Un composant structurel d’interface utilisateur requis sur chaque page web Parts est le contrôle WebPartManager. Bien que jamais visible, ce contrôle a la tâche critique de coordonner tous les contrôles de pièces Web sur une page. Par exemple, il suit tous les contrôles individuels de parties Web. Il gère les zones de pièces Web (régions qui contiennent des contrôles de pièces Web sur une page), et quels contrôles sont dans quelles zones. Il suit et contrôle également les différents modes d’affichage dans lequel une page peut être, comme naviguer, connecter, modifier ou cataloguer le mode, et si des modifications de personnalisation s’appliquent à tous les utilisateurs ou aux utilisateurs individuels. Enfin, il initie et suit les connexions et la communication entre les contrôles Web Parts.

Le deuxième type de composante structurelle de l’interface utilisateur est la zone. Les zones agissent en tant que gestionnaires de mise en page sur une page Web Parts. Ils contiennent et organisent des contrôles qui dérivent de la classe Part (contrôles de partie), et offrent la possibilité de faire la disposition modulaire de la page dans l’orientation horizontale ou verticale. Les zones offrent également des éléments d’interface utilisateur communs et cohérents (tels que le style en-tête et le pied, le titre, le style de bordure, les boutons d’action, et ainsi de suite) pour chaque contrôle qu’ils contiennent; ces éléments communs sont connus comme le chrome d’un contrôle. Plusieurs types spécialisés de zones sont utilisés dans les différents modes d’affichage et avec différents contrôles. Les différents types de zones sont décrits dans la section Des parties Web essential Controls ci-dessous.

Les contrôles d’interface utilisateur De pièces Web, qui proviennent tous de la classe **Part,** comprennent l’interface utilisateur principale sur une page Web Parts. L’ensemble de contrôle Web Parts est flexible et inclusif dans les options qu’il vous offre pour créer des contrôles de pièces. En plus de créer vos propres contrôles Personnalisés de parties Web, vous pouvez également utiliser les contrôles de serveur ASP.NET existants, les contrôles utilisateur ou les contrôles personnalisés des serveurs sous forme de contrôles Web Parts. Les contrôles essentiels les plus couramment utilisés pour créer des pages Web Parts sont décrits dans la section suivante.

## <a name="web-parts-essential-controls"></a>Pièces Web Contrôles essentiels

L’ensemble de contrôle des parties Web est étendu, mais certains contrôles sont essentiels, soit parce qu’ils sont nécessaires pour que les parties Web fonctionnent, soit parce qu’ils sont les contrôles les plus fréquemment utilisés sur les pages Web Parts. Lorsque vous commencez à utiliser des parties Web et à créer des pages Web Parts de base, il est utile de connaître les contrôles essentiels des parties Web décrits dans le tableau suivant.

| **Contrôle WebPart** | **Description** |
| --- | --- |
| Webpartmanager | Gère tous les contrôles de pièces Web sur une page. Un (et un seul) contrôle **WebPartManager** est nécessaire pour chaque page Web Parts. |
| CatalogueZone | Contient des commandes CatalogPart. Utilisez cette zone pour créer un catalogue de contrôles Web Parts à partir duquel les utilisateurs peuvent sélectionner des contrôles à ajouter à une page. |
| EditorZone (en) | Contient des commandes EditorPart. Utilisez cette zone pour permettre aux utilisateurs de modifier et de personnaliser les contrôles de pièces Web sur une page. |
| Webpartzone | Contient et fournit une mise en page globale pour les contrôles WebPart qui composent l’interface utilisateur principale d’une page. Utilisez cette zone chaque fois que vous créez des pages avec des contrôles Web Parts. Les pages peuvent contenir une ou plusieurs zones. |
| ConnexionsZone | Contient des contrôles WebPartConnection et fournit une interface utilisateur pour la gestion des connexions. |
| WebPart (GenericWebPart) | Rend l’interface utilisateur primaire; la plupart des contrôles d’interface utilisateur de pièces Web entrent dans cette catégorie. Pour un contrôle programmatique maximum, vous pouvez créer des contrôles Web Parts personnalisés qui dérivent du contrôle **WebPart** de base. Vous pouvez également utiliser les contrôles de serveur existants, les contrôles utilisateur ou les contrôles personnalisés sous forme de contrôles Web Parts. Chaque fois que l’un de ces contrôles est placé dans une zone, le contrôle **WebPartManager** les enveloppe automatiquement avec des commandes **GenericWebPart** à l’heure d’exécution afin que vous puissiez les utiliser avec la fonctionnalité Web Parts. |
| CataloguePart | Contient une liste des contrôles web disponibles que les utilisateurs peuvent ajouter à la page. |
| WebPartConnection (en) | Crée une connexion entre deux contrôles Web Parts sur une page. La connexion définit l’un des contrôles Web Parts comme un fournisseur (de données), et l’autre en tant que consommateur. |
| Editorpart | Sert de classe de base pour les commandes de l’éditeur spécialisé. |
| Contrôles EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart et PropertyGridEditorPart) | Permettre aux utilisateurs de personnaliser divers aspects des contrôles d’interface utilisateur De pièces Web sur une page |

## <a name="lab-create-a-web-part-page"></a>Laboratoire : Créer une page Web Partie

Dans ce laboratoire, vous créerez une page Web qui persistera l’information via ASP.NET profils.

### <a name="creating-a-simple-page-with-web-parts"></a>Création d’une page simple avec des pièces Web

Dans cette partie de la procédure pas à pas, vous créez une page qui utilise les contrôles Web Parts pour afficher du contenu statique. La première étape dans le travail avec Web Parts est de créer une page avec deux éléments structurels nécessaires. Tout d’abord, une page de parties Web a besoin d’un contrôle WebPartManager pour suivre et coordonner tous les contrôles de pièces Web. Deuxièmement, une page Web Parts a besoin d’une ou de plusieurs zones, qui sont des contrôles composites qui contiennent des contrôles WebPart ou d’autres serveurs et occupent une région spécifiée d’une page.

> [!NOTE]
> Vous n’avez pas besoin de faire quoi que ce soit pour permettre la personnalisation des parties Web; il est activé par défaut pour l’ensemble de contrôle Web Parts. Lorsque vous exécutez pour la première fois une page Web Parts sur un site, ASP.NET crée un fournisseur de personnalisation par défaut pour stocker les paramètres de personnalisation des utilisateurs. Pour plus d’informations sur la personnalisation, consultez l’aperçu de la personnalisation des parties Web.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Créer une page pour contenir les contrôles des pièces Web

1. Fermez la page par défaut et ajoutez une nouvelle page au site nommé WebPartsDemo.aspx.
2. Passez à la vue **Design.**
3. À partir du menu **View,** **assurez-vous** que les options de contrôles et **de détails** non visuels sont sélectionnées afin que vous puissiez voir les balises de mise en page et les contrôles qui n’ont pas d’interface utilisateur.
4. Placez le point `<div>` d’insertion avant les balises sur la surface de conception, et appuyez sur ENTER pour ajouter une nouvelle ligne. Placez le point d’insertion avant le nouveau personnage de ligne, cliquez sur le contrôle de liste d’abandon **block Format** sur le menu et sélectionnez **l’option Heading 1.** Dans le titre, ajoutez le texte **Web Parts Demonstration Page**.
5. De l’onglet **WebParts** de la boîte à outils, faites glisser un contrôle **WebPartManager** `<div>`sur la page, en la positionnant juste après le nouveau personnage de ligne et avant les balises.   
  
   Le contrôle **WebPartManager** ne rend aucune sortie, de sorte qu’il apparaît comme une boîte grise sur la surface du concepteur.
6. Placez le point `<div>` d’insertion dans les balises.
7. Dans le menu **Layout,** cliquez sur **Insert Table**, et créez une nouvelle table qui a une rangée et trois colonnes. Cliquez sur le bouton **Propriétés cellulaires,** sélectionnez **en haut** de la liste d’abandon **vertical align,** cliquez **sur OK**, et **cliquez** à nouveau pour créer la table.
8. Faites glisser un contrôle WebPartZone dans la colonne de la table gauche. Cliquez à droite sur le contrôle **WebPartZone,** choisissez **les propriétés**et définissez les propriétés suivantes :   
  
   ID: SidebarZone   
  
   HeaderText: Barre latérale
9. Faites glisser un deuxième contrôle **WebPartZone** dans la colonne de table du milieu et définissez les propriétés suivantes :   
  
   ID: MainZone   
  
   HeaderText: Main
10. Enregistrez le fichier .

Votre page dispose désormais de deux zones distinctes que vous pouvez contrôler séparément. Cependant, aucune des deux zones n’a de contenu, donc la création de contenu est la prochaine étape. Pour cette procédure pas à pas, vous travaillez avec des contrôles Web Parts qui n’affichent que du contenu statique.

La disposition d’une zone de &lt;parties Web&gt; est spécifiée par un élément zonetemplate. À l’intérieur du modèle de zone, vous pouvez ajouter n’importe quel contrôle ASP.NET, qu’il s’agisse d’un contrôle personnalisé des parties Web, d’un contrôle utilisateur ou d’un contrôle serveur existant. Notez qu’ici vous utilisez le contrôle d’étiquette, et à ce que vous êtes simplement ajouter du texte statique. Lorsque vous placez un contrôle serveur régulier dans une zone **WebPartZone,** ASP.NET traite le contrôle comme un contrôle des parties Web au moment de l’exécution, ce qui permet aux fonctionnalités web Parts sur le contrôle.

**Créer du contenu pour la zone principale**

1. Dans la vue **Design,** faites glisser un contrôle **d’étiquette** de l’onglet **Standard** de la boîte à outils dans la zone de contenu de la zone dont la propriété **ID** est réglée à MainZone.
2. Passez à la vue **Source.** Notez &lt;qu’un&gt; élément zonetemplate a été ajouté pour envelopper le contrôle **de l’étiquette** dans la zone principale.
3. Ajoutez un **titre** nommé &lt;attribut à&gt; l’élément asp:label, et définissez sa valeur au contenu. Supprimer l’attribut TextMD "Label" de l’élément &lt;asp:label.&gt; Entre les balises &lt;d’ouverture&gt; et de fermeture de l’élément asp:label,&gt; ajoutez un peu de texte comme Bienvenue sur ma page **d’accueil** dans une paire de &lt;balises d’élément h2. Votre code doit ressembler à ce qui suit. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Enregistrez le fichier .

Ensuite, créez un contrôle utilisateur qui peut également être ajouté à la page sous forme de contrôle des parties Web.

### <a name="to-create-a-user-control"></a>Pour créer un contrôle utilisateur

1. Ajoutez un nouveau contrôle utilisateur Web à votre site pour servir de contrôle de recherche. Désélectionner l’option de placer le **code source dans un fichier séparé**. Ajoutez-le dans le même répertoire que la page WebPartsDemo.aspx, et nommez-le SearchUserControl.ascx.   
  
    > [!NOTE]
    > Le contrôle de l’utilisateur pour cette procédure pas à pas ne met pas en œuvre la fonctionnalité de recherche réelle; il est utilisé uniquement pour démontrer les fonctionnalités web Parts.
2. Passez à la vue **Design.** De l’onglet **Standard** de la boîte à outils, faites glisser un contrôle TextBox sur la page.
3. Placez le point d’insertion après la boîte de texte que vous venez d’ajouter, et appuyez sur ENTER pour ajouter une nouvelle ligne.
4. Faites glisser un contrôle de bouton sur la page sur la nouvelle ligne ci-dessous la boîte de texte que vous venez d’ajouter.
5. Passez à la vue **Source.** Assurez-vous que le code source pour le contrôle de l’utilisateur ressemble à l’exemple suivant. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Enregistrez et fermez le fichier.

Vous pouvez maintenant ajouter des commandes de pièces Web à la zone Sidebar. Vous ajoutez deux contrôles à la zone Sidebar, l’un contenant une liste de liens et l’autre qui est le contrôle utilisateur que vous avez créé dans la procédure précédente. Les liens sont ajoutés comme un contrôle standard du serveur **Label,** similaire à la façon dont vous avez créé le texte statique pour la zone principale. Cependant, bien que les contrôles de serveur individuels contenus dans le contrôle de l’utilisateur puissent être contenus directement dans la zone (comme le contrôle de l’étiquette), dans ce cas, ils ne sont pas. Au lieu de cela, ils font partie du contrôle de l’utilisateur que vous avez créé dans la procédure précédente. Cela démontre un moyen commun d’emballer les contrôles et les fonctionnalités supplémentaires que vous souhaitez dans un contrôle utilisateur, puis de référence que le contrôle dans une zone comme un contrôle des pièces Web.

Au moment de l’exécution, l’ensemble de contrôle Des parties Web enveloppe les deux contrôles avec les commandes GenericWebPart. Lorsqu’un contrôle **GenericWebPart** enveloppe un contrôle de serveur Web, le contrôle générique de la partie est le contrôle parent, et vous pouvez accéder au contrôle du serveur via la propriété ChildControl du contrôle parent. Cette utilisation de contrôles de pièces génériques permet aux contrôles standard du serveur Web d’avoir le même comportement et attributs de base que les contrôles Web Parts qui dérivent de la classe **WebPart.**

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Pour ajouter des commandes de pièces Web à la zone de la barre latérale

1. Ouvrez la page WebPartsDemo.aspx.
2. Passez à la vue **Design.**
3. Faites glisser la page de contrôle utilisateur que vous avez créée, SearchUserControl.ascx, de **Solution Explorer** dans la zone dont la propriété **ID** est réglée à SidebarZone, et déposez-la là.
4. Enregistrer la page WebPartsDemo.aspx.
5. Passez à la vue **Source.**
6. À &lt;l’intérieur de&gt; l’élément asp:webpartzone pour la SidebarZone, juste au-dessus de la référence à votre contrôle utilisateur, ajoutez un &lt;élément d’étiquette&gt; asp: avec des liens contenus, comme indiqué dans l’exemple suivant. En outre, ajoutez un attribut **de titre** à l’étiquette de contrôle de l’utilisateur, avec une valeur de **recherche**, comme indiqué. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Enregistrez et fermez le fichier.

Maintenant, vous pouvez tester votre page en naviguant vers elle dans votre navigateur. La page affiche les deux zones. La capture d’écran suivante montre la page.

**Page démo de parties Web avec deux zones**

![Web Parts VS Procédure pas à pas 1 Capture d’écran](profiles-themes-and-web-parts/_static/image3.gif)

**Figure 3**: Web Parts VS Procédure pas à pas 1 Capture d’écran

Dans la barre de titre de chaque contrôle est une flèche vers le bas qui donne accès à un menu verbes d’actions disponibles, vous pouvez effectuer sur un contrôle. Cliquez sur le menu verbes pour l’un des contrôles, puis cliquez sur le verbe **Minimiser** et notez que le contrôle est minimisé. Du menu verbes, cliquez sur **Restaurer,** et le contrôle revient à sa taille normale.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Permettre aux utilisateurs d’éditer des pages et de modifier la mise en page

Web Parts offre aux utilisateurs la possibilité de modifier la disposition des contrôles des parties Web en les faisant glisser d’une zone à l’autre. En plus de permettre aux utilisateurs de déplacer les contrôles **WebPart** d’une zone à l’autre, vous pouvez permettre aux utilisateurs de modifier diverses caractéristiques des contrôles, y compris leur apparence, mise en page et leur comportement. L’ensemble de contrôle Web Parts fournit des fonctionnalités d’édition de base pour les contrôles **WebPart.** Bien que vous ne le fassiez pas dans cette procédure pas à pas, vous pouvez également créer des commandes d’éditeur personnalisées qui permettent aux utilisateurs de modifier les fonctionnalités des contrôles **WebPart.** Comme pour changer l’emplacement d’un contrôle **WebPart,** l’édition des propriétés d’un contrôle repose sur ASP.NET personnalisation pour enregistrer les modifications apportées par les utilisateurs.

Dans cette partie de la procédure pas à pas, vous ajoutez la possibilité pour les utilisateurs de modifier les caractéristiques de base de tout contrôle **WebPart** sur la page. Pour activer ces fonctionnalités, vous ajoutez un autre &lt;contrôle personnalisé&gt; de l’utilisateur à la page, ainsi qu’un élément asp:editorzone et deux contrôles d’édition.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Créer un contrôle utilisateur qui permet de changer la mise en page

1. Dans Visual Studio, sur le menu **Fichier,** sélectionnez le **nouveau** sous-mois et cliquez sur **l’option Fichier.**
2. Dans le dialogue **Add New Item,** sélectionnez **Le contrôle des utilisateurs Web**. Nommez le nouveau fichier DisplayModeMenu.ascx. Désélectionner l’option de placer le **code source dans un fichier séparé**.
3. Cliquez sur Ajouter pour créer le nouveau contrôle.
4. Passez à la vue **Source.**
5. Supprimer tout le code existant dans le nouveau fichier, et coller dans le code suivant. Ce code de contrôle utilisateur utilise des fonctionnalités de l’ensemble de contrôle des parties Web qui permettent à une page de modifier sa vue ou son mode d’affichage, et vous permet également de modifier l’apparence physique et la disposition de la page lorsque vous êtes dans certains modes d’affichage. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Enregistrez le fichier en cliquant sur l’icône d’enregistrement sur la barre d’outils, ou en sélectionnant **Enregistrer** sur le menu **Fichier.**

### <a name="to-enable-users-to-change-the-layout"></a>Pour permettre aux utilisateurs de modifier la mise en page

1. Ouvrez la page WebPartsDemo.aspx, et passez à la vue **Design.**
2. Placez le point d’insertion dans la vue **de conception** juste après le contrôle **WebPartManager** que vous avez ajouté plus tôt. Ajoutez un retour difficile après le texte afin qu’il y ait une ligne blanche après le contrôle **WebPartManager.** Placez le point d’insertion sur la ligne blanche.
3. Faites glisser le contrôle utilisateur que vous venez de créer (le fichier est nommé DisplayModeMenu.ascx) dans la page WebPartsDemo.aspx et laissez-le tomber sur la ligne blanche.
4. Faites glisser un contrôle EditorZone de la section **WebParts** de la boîte à outils à la cellule de table ouverte restante dans la page WebPartsDemo.aspx.
5. De la section **WebParts** de la boîte à outils, faites glisser un contrôle AppearanceEditorPart et un contrôle LayoutEditorPart dans le contrôle **EditorZone.**
6. Passez à la vue **Source.** Le code résultant dans la cellule de table doit ressembler au code suivant. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Enregistrer le fichier WebPartsDemo.aspx. Vous avez créé un contrôle utilisateur qui vous permet de modifier les modes d’affichage et de modifier la mise en page, et vous avez fait référence au contrôle sur la page Web principale.

Vous pouvez maintenant tester la capacité de modifier les pages et de modifier la mise en page.

### <a name="to-test-layout-changes"></a>Pour tester les modifications de la mise en page

1. Chargez la page dans un navigateur.
2. Cliquez sur le menu **déroulant du mode Display** et sélectionnez **Edit**. Les titres de zone sont affichés.
3. Faites glisser le contrôle **My Links** par sa barre de titre de la zone Sidebar au bas de la zone principale. Votre page devrait ressembler à la capture d’écran suivante.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Page démo de parties Web avec my Links control déplacé

![Web Parts VS Procédure pas à pas 2 Capture d’écran](profiles-themes-and-web-parts/_static/image4.gif)

**Figure 4**: Web Parts VS Procédure pas à pas 2 Capture d’écran

1. Cliquez sur le menu **déroulant du mode Display** et **sélectionnez Parcourir**. La page est actualisée, les noms de zone disparaissent, et le contrôle **My Links** reste là où vous l’avez positionné.
2. Pour démontrer que la personnalisation fonctionne, fermez le navigateur, puis chargez la page à nouveau. Les modifications que vous avez apportées sont enregistrées pour les futures sessions de navigateur.
3. Dans le menu **Mode Display,** sélectionnez **Edit**.   
  
   Chaque contrôle sur la page est maintenant affiché avec une flèche vers le bas dans sa barre de titre, qui contient le menu de dépôt de verbes.
4. Cliquez sur la flèche pour afficher le menu verbes sur le contrôle **My Links.** Cliquez sur le verbe **Edit.**   
  
   Le contrôle **EditorZone** apparaît, affichant les commandes EditorPart que vous avez ajouté.
5. Dans la section **Apparence** du contrôle d’édition, modifiez le **titre** en mes favoris, utilisez la liste d’abandon du **type Chrome** pour sélectionner le **titre uniquement,** puis cliquez sur **Apply**. La capture d’écran suivante affiche la page en mode d’édition.

### <a name="web-parts-demo-page-in-edit-mode"></a>Page de démonstration de parties Web en mode Modifier

![Web Parts VS Procédure pas à pas 3 Capture d’écran](profiles-themes-and-web-parts/_static/image5.gif)

**Figure 5**: Web Parts VS Procédure pas à pas 3 Capture d’écran

1. Cliquez sur le menu **Mode Display,** et **sélectionnez Parcourir** pour revenir en mode de navigation.
2. Le contrôle a maintenant un titre mis à jour et aucune bordure, comme indiqué dans la capture d’écran suivante.

### <a name="edited-web-parts-demo-page"></a>Page de démonstration de pièces Web éditées

![Web Parts VS Procédure pas à pas 4 Capture d’écran](profiles-themes-and-web-parts/_static/image6.gif)

**Figure 4**: Web Parts VS Procédure pas à pas 4 Capture d’écran

### <a name="adding-web-parts-at-run-time"></a>Ajout de pièces Web à l’heure de course

Vous pouvez également permettre aux utilisateurs d’ajouter des contrôles de pièces Web à leur page à l’heure de l’exécution. Pour ce faire, configurez la page avec un catalogue De parties Web, qui contient une liste de contrôles De pièces Web que vous souhaitez mettre à la disposition des utilisateurs.

**Permettre aux utilisateurs d’ajouter des pièces Web au moment de l’exécution**

1. Ouvrez la page WebPartsDemo.aspx, et passez à la vue **Design.**
2. De l’onglet **WebParts** de la boîte à outils, faites glisser un contrôle CatalogZone dans la colonne droite de la table, sous le contrôle **EditorZone.**   
  
   Les deux contrôles peuvent être dans la même cellule de table parce qu’ils ne seront pas affichés en même temps.
3. Dans le volet Propriétés, attribuez la chaîne **Ajouter des parties Web** à la propriété HeaderText du contrôle **CatalogZone.**
4. De la section **WebParts** de la boîte à outils, faites glisser un contrôle DeclarativeCatalogPart dans la zone de contenu du contrôle **CatalogZone.**
5. Cliquez sur la flèche dans le coin supérieur droit du contrôle **DeclarativeCatalogPart** pour exposer son menu Tasks, puis sélectionnez **Edit Templates**.
6. De la section **Standard** de la Boîte à outils, faites glisser un contrôle **FileUpload** et un contrôle **de calendrier** dans la section **WebPartsTemplate** du contrôle **DeclarativeCatalogPart.**
7. Passez à la vue **Source.** Inspectez le code &lt;source de&gt; l’élément asp:catalogzone. Notez que le contrôle **DeclarativeCatalogPart** contient un &lt;élément webpartstemplate&gt; avec les deux contrôles de serveur fermés que vous serez en mesure d’ajouter à votre page à partir du catalogue.
8. Ajoutez une propriété **Titre** à chacun des contrôles que vous avez ajoutés au catalogue, en utilisant la valeur de chaîne indiquée pour chaque titre dans l’exemple de code ci-dessous. Même si le titre n’est pas une propriété que vous pouvez normalement définir sur ces deux contrôles de serveur au moment de la conception, quand un utilisateur ajoute ces contrôles à une zone **WebPartZone** à partir du catalogue à l’heure d’exécution, ils sont chacun enveloppé avec un contrôle **GenericWebPart.** Cela leur permet d’agir comme des contrôles Web Parts, de sorte qu’ils seront en mesure d’afficher des titres.   
  
   Le code pour les deux contrôles contenus dans le contrôle **DeclarativeCatalogPart** doit ressembler à ce qui suit. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Enregistrez la page.

Vous pouvez maintenant tester le catalogue.

### <a name="to-test-the-web-parts-catalog"></a>Pour tester le catalogue Web Parts

1. Chargez la page dans un navigateur.
2. Cliquez sur le menu **déroulant du mode d’affichage** et sélectionnez **Catalogue**.   
  
   Le catalogue intitulé **Add Web Parts** est affiché.
3. Faites glisser le contrôle **My Favorites** de la zone principale vers le haut de la zone Sidebar, et déposez-le là.
4. Dans le catalogue **Add Web Parts,** sélectionnez les deux cases à cocher, puis sélectionnez **Main** dans la liste d’abandon qui contient les zones disponibles.
5. Cliquez **sur Ajouter** dans le catalogue. Les commandes sont ajoutées à la zone principale. Si vous le souhaitez, vous pouvez ajouter plusieurs instances de contrôles du catalogue à votre page.   
  
   La capture d’écran suivante affiche la page avec le contrôle de téléchargement de fichier et le calendrier dans la zone principale. 

![Contrôles ajoutés à la zone principale à partir du catalogue](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Cliquez sur le menu **déroulant du mode Display** et **sélectionnez Parcourir**. Le catalogue disparaît et la page est actualisée.
7. Fermez le navigateur. Chargez à nouveau la page. Les changements que vous avez apportés persistent.
