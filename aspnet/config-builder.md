---
uid: config-builder
title: Générateurs de configuration pour ASP.NET
author: rick-anderson
description: Découvrez comment obtenir des données de configuration à partir de sources autres que les valeurs web.config provenant de sources externes.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 443b33b5c3b964f731999834db580a6abbf6617b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59420417"
---
# <a name="configuration-builders-for-aspnet"></a>Générateurs de configuration pour ASP.NET

Par [Stephen Molloy](https://github.com/StephenMolloy) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Générateurs de configuration fournissent un mécanisme modern et agile pour les applications ASP.NET obtenir les valeurs de configuration à partir de sources externes.

Générateurs de configuration :

* Sont disponibles dans .NET Framework 4.7.1 et versions ultérieures.
* Fournir un mécanisme flexible pour la lecture des valeurs de configuration.
* Résoudre certains besoins fondamentaux des applications dans leur déplacement dans un conteneur et environnement ayant le focus de cloud.
* Peut être utilisé pour améliorer la protection des données de configuration par dessin à partir de sources précédemment pas disponibles (par exemple, variables d’environnement et d’Azure Key Vault) dans le système de configuration .NET.

## <a name="keyvalue-configuration-builders"></a>Générateurs de configuration clé/valeur

Un scénario courant qui peut être géré par les générateurs de configuration est de fournir un mécanisme de remplacement de clé/valeur de base pour les sections de configuration qui suivent un modèle de clé/valeur. Le concept de .NET Framework de ConfigurationBuilders n’est pas limité aux sections de configuration spécifiques ou des modèles. Toutefois, un grand nombre des générateurs de configuration dans `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) professionnel dans le modèle de clé/valeur.

## <a name="keyvalue-configuration-builders-settings"></a>Paramètres de générateurs de configuration de clé/valeur

Les paramètres suivants s’appliquent à tous les générateurs de configuration de clé/valeur dans `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Mode

Les générateurs de configuration utilisent une source externe d’informations de clé/valeur pour remplir les éléments de clé/valeur sélectionnée du système de configuration. Plus précisément, le `<appSettings/>` et `<connectionStrings/>` sections reçoivent un traitement spécial dans les générateurs de configuration. Les générateurs de fonctionnent dans trois modes :

* `Strict` -Le mode par défaut. Dans ce mode, le Générateur de configuration ne fonctionne que sur les sections de configuration clé/valeur-centric bien connu. `Strict` mode énumère chaque clé dans la section. Si une clé correspondante est trouvée dans la source externe :

   * Les générateurs de configuration remplacement la valeur dans la section de configuration qui en résulte par la valeur de la source externe.
* `Greedy` -Ce mode est étroitement lié à `Strict` mode. Au lieu d’être limité aux clés qui existent déjà dans la configuration d’origine :

  * Les générateurs de configuration ajoute toutes les paires clé/valeur à partir de la source externe dans la section de configuration qui en résulte.

* `Expand` -Opère sur le code XML brut avant qu’il est analysé dans un objet de section de configuration. Elle peut être considérée comme expansion de jetons présents dans une chaîne. N’importe quelle partie de la chaîne XML brute qui correspond au modèle `${token}` est un candidat pour l’expansion de jeton. Si aucune valeur correspondante n’est trouvée dans la source externe, le jeton n’est pas modifié. Générateurs dans ce mode ne sont pas limités à la `<appSettings/>` et `<connectionStrings/>` sections.

Le balisage suivant à partir de *web.config* permet la [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) dans `Strict` mode :

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Le code suivant lit le `<appSettings/>` et `<connectionStrings/>` indiqué dans l’exemple précédent *web.config* fichier :

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Le code précédent définit les valeurs de propriété :

* Les valeurs dans le *web.config* fichier si les clés ne sont pas définies dans les variables d’environnement.
* Les valeurs de l’environnement variable, si définie.

Par exemple, `ServiceID` contiendra :

* « ServiceID valeur à partir de web.config », si la variable d’environnement `ServiceID` n’est pas définie.
* La valeur de la `ServiceID` if variable d’environnement définie.

L’illustration suivante montre le `<appSettings/>` clés/valeurs à partir de l’exemple précédent *web.config* jeu de fichiers dans l’éditeur de l’environnement :

![éditeur de l’environnement](config-builder/static/env.png)

Remarque : Vous devrez peut-être quitter et redémarrer Visual Studio pour afficher les modifications dans les variables d’environnement.

### <a name="prefix-handling"></a>Gestion de préfixe

Préfixes de clé peuvent simplifier les clés de paramètres, car :

* La configuration de .NET Framework est complexe et imbriqués.
* Sources de clé/valeur externes sont couramment basiques et plat par nature. Par exemple, les variables d’environnement ne sont pas imbriqués.

Utiliser une des approches suivantes pour injecter à la fois `<appSettings/>` et `<connectionStrings/>` dans la configuration par le biais de variables d’environnement :

* Avec le `EnvironmentConfigBuilder` dans la valeur par défaut `Strict` mode et les noms de clé appropriés dans le fichier de configuration. Le code et le balisage précédent adopte cette approche. À l’aide de cette approche, vous pouvez **pas** ont un nom identique dans les deux clés de `<appSettings/>` et `<connectionStrings/>`.
* Utilisez deux `EnvironmentConfigBuilder`s dans `Greedy` mode avec des préfixes distincts et `stripPrefix`. Avec cette approche, l’application peut lire `<appSettings/>` et `<connectionStrings/>` sans avoir à mettre à jour le fichier de configuration. La section suivante, [stripPrefix](#stripprefix), montre comment effectuer cette opération.
* Utilisez deux `EnvironmentConfigBuilder`s dans `Greedy` mode avec des préfixes distincts. Avec cette approche, vous ne pouvez avoir des noms de clé en double comme noms de clés doivent différer par préfixe.  Exemple :

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Avec le balisage précédent, la même source de clé-valeur plates peut être utilisée pour remplir la configuration pour les deux sections différentes.

L’illustration suivante montre le `<appSettings/>` et `<connectionStrings/>` clés/valeurs à partir de l’exemple précédent *web.config* jeu de fichiers dans l’éditeur de l’environnement :

![éditeur de l’environnement](config-builder/static/prefix.png)

Le code suivant lit le `<appSettings/>` et `<connectionStrings/>` clés/valeurs contenues dans l’exemple précédent *web.config* fichier :

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Le code précédent définit les valeurs de propriété :

* Les valeurs dans le *web.config* fichier si les clés ne sont pas définies dans les variables d’environnement.
* Les valeurs de l’environnement variable, si définie.

Par exemple, à l’aide de la précédente *web.config* fichier, les clés/valeurs dans l’image de l’éditeur environnement précédente et le code précédent, les valeurs suivantes sont définies :

|  Touche              | Value |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID à partir de variables d’env|
|    AppSetting_default            | Valeur de AppSetting_default à partir d’env |
|       ConnStr_default         | Val ConnStr_default à partir d’env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: valeur booléenne, valeur par défaut est `false`. 

Le balisage XML précédent sépare les paramètres de l’application à partir de chaînes de connexion, mais nécessite toutes les clés dans le *web.config* fichier à utiliser le préfixe spécifié. Par exemple, le préfixe `AppSetting` doit être ajouté à la `ServiceID` clé (« AppSetting_ServiceID »). Avec `stripPrefix`, le préfixe n’est pas utilisé dans le *web.config* fichier. Le préfixe est requis dans la source du Générateur de configuration (par exemple, dans l’environnement.) Nous pensons que la plupart des développeurs utilisera `stripPrefix`.

Applications suppriment généralement le préfixe. Ce qui suit *web.config* supprime le préfixe :

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

Dans l’exemple précédent *web.config* fichier, le `default` clé se trouve dans les deux le `<appSettings/>` et `<connectionStrings/>`.

L’illustration suivante montre le `<appSettings/>` et `<connectionStrings/>` clés/valeurs à partir de l’exemple précédent *web.config* jeu de fichiers dans l’éditeur de l’environnement :

![éditeur de l’environnement](config-builder/static/prefix.png)

Le code suivant lit le `<appSettings/>` et `<connectionStrings/>` clés/valeurs contenues dans l’exemple précédent *web.config* fichier :

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Le code précédent définit les valeurs de propriété :

* Les valeurs dans le *web.config* fichier si les clés ne sont pas définies dans les variables d’environnement.
* Les valeurs de l’environnement variable, si définie.

Par exemple, à l’aide de la précédente *web.config* fichier, les clés/valeurs dans l’image de l’éditeur environnement précédente et le code précédent, les valeurs suivantes sont définies :

|  Touche              | Value |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID à partir de variables d’env|
|    default            | Valeur de AppSetting_default à partir d’env |
|    default         | Val ConnStr_default à partir d’env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Chaîne, valeur par défaut est `@"\$\{(\w+)\}"`

Le `Expand` comportement des générateurs de recherche dans le code XML brut pour les jetons qui ressemblent à `${token}`. La recherche est effectuée avec l’expression régulière par défaut `@"\$\{(\w+)\}"`. Le jeu de caractères qui correspond à `\w` est plus stricte que XML et nombreuses sources de configuration ne l’autorisent. Utilisez `tokenPattern` lorsque plus de caractères que `@"\$\{(\w+)\}"` sont requis dans le nom du jeton.

`tokenPattern`: Chaîne :

* Permet aux développeurs de modifier l’expression régulière qui est utilisé pour la correspondance de jeton.
* Aucune validation n’est effectuée pour vous assurer qu’il est une expression régulière bien formée, non dangereux.
* Il doit contenir un groupe de capture. L’expression régulière entière doit correspondre au jeton entier. La première capture doit être le nom du jeton à rechercher dans la source de configuration.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Générateurs de configuration dans Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

Le [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Est la plus simple des générateurs de configuration.
* Lit les valeurs à partir de l’environnement.
* Ne dispose pas des options de configuration supplémentaires.
* Le `name` valeur d’attribut est arbitraire.

**Remarque :** Dans un environnement de conteneur Windows, les variables définies au moment de l’exécution sont uniquement injectés dans l’environnement de processus de point d’entrée. Les applications qui s’exécutent en tant que service ou un processus non EntryPoint ne récupèrent pas ces variables, sauf si elles sont injectées dans le cas contraire via un mécanisme dans le conteneur. Pour [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-en fonction des conteneurs, la version actuelle de [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) gère cela dans le *DefaultAppPool* uniquement. Autres variantes de conteneur basé sur Windows devrez peut-être développer leur propre mécanisme d’injection de code pour les processus non EntryPoint.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Jamais magasin de mots de passe, chaînes de connexion sensibles ou autres données sensibles dans le code source. Aucun secret de production ne doit pas être utilisé pour le développement ou de test.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Ce générateur de configuration fournit une fonctionnalité similaire à [Secret Manager d’ASP.NET Core](/aspnet/core/security/app-secrets).

Le [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) peut être utilisé dans les projets .NET Framework, mais un fichier de secrets doit être spécifié. Vous pouvez également définir le `UserSecretsId` propriété dans le projet fichiers et de créer le fichier de secrets brutes à l’emplacement approprié pour la lecture. Pour conserver les dépendances externes en dehors de votre projet, le fichier de secret est au format XML. La mise en forme XML est un détail d’implémentation, et le format ne doit pas se reposer. Si vous avez besoin de partager un *secrets.json* de fichiers avec les projets .NET Core, envisagez d’utiliser le [SimpleJsonConfigBuilder](#simplejsonconfigbuilder). Le `SimpleJsonConfigBuilder` mettre en forme pour .NET Core doit également être considéré comme un détail d’implémentation susceptibles d’être modifiées.

Les attributs de configuration de `UserSecretsConfigBuilder`:

* `userSecretsId` -Il s’agit de la méthode recommandée pour l’identification d’un fichier de secrets XML. Il est similaire à .NET Core, qui utilise un `UserSecretsId` propriété pour stocker cet identificateur du projet. La chaîne doit être unique, il n’a pas besoin être un GUID. Avec cet attribut, le `UserSecretsConfigBuilder` Regarder dans un emplacement local connu (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) pour un fichier de secrets appartenant à cet identificateur.
* `userSecretsFile` -Attribut facultatif spécifiant le fichier contenant les clés secrètes. Le `~` caractère peut être utilisé au début pour faire référence à la racine de l’application. Cet attribut ou le `userSecretsId` attribut est requis. Si les deux sont spécifiés, `userSecretsFile` est prioritaire.
* `optional`: boolean, valeur par défaut `true` -empêche une exception si le fichier de secrets ne peut pas être trouvé. 
* Le `name` valeur d’attribut est arbitraire.

Le fichier de secrets a le format suivant :

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

Le [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lit les valeurs stockées dans le [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` est requis (le nom du coffre) ou un URI dans le coffre. Les autres attributs permettent un contrôle sur le coffre pour se connecter à, mais sont nécessaires seulement si l’application n’est pas en cours d’exécution dans un environnement qui fonctionne avec `Microsoft.Azure.Services.AppAuthentication`. La bibliothèque d’authentification des Services Azure est utilisée pour choisir automatiquement les informations de connexion à partir de l’environnement d’exécution si possible. Vous pouvez remplacer automatiquement récupérer les informations de connexion en fournissant une chaîne de connexion.

* `vaultName` -Obligatoire si `uri` dans ne pas fourni. Spécifie le nom du coffre dans votre abonnement Azure à partir duquel lire les paires clé/valeur.
* `connectionString` -Une chaîne de connexion utilisable par [le paramètre AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -Se connecte à d’autres fournisseurs de Key Vault avec la valeur `uri` valeur. Si non spécifié, Azure (`vaultName`) est le fournisseur du coffre.
* `version` -Azure Key Vault fournit une fonctionnalité de contrôle de version pour les clés secrètes. Si `version` est spécifié, le générateur récupère uniquement les secrets correspondant à cette version.
* `preloadSecretNames` -Par défaut, ce générateur de requêtes **tous les** dans le coffre de clés, les noms de clé lors de son initialisation. Pour empêcher la lecture de toutes les valeurs de clés, définissez cet attribut sur `false`. Définir cette valeur sur `false` lit les secrets celui à la fois. Lecture de secrets à la fois est utile si le coffre autorise l’accès « Get » mais pas « liste » accéder. **Remarque :** Lorsque vous utilisez `Greedy` mode, `preloadSecretNames` doit être `true` (la valeur par défaut.)

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) est un générateur de configuration de base qui utilise les fichiers d’un répertoire en tant que source de valeurs. Nom d’un fichier est la clé, et le contenu est la valeur. Ce générateur de configuration peut être utile lors de l’exécution dans un environnement de conteneur orchestrée. Systèmes, comme Docker Swarm et Kubernetes fournissent `secrets` à leurs conteneurs orchestrée windows de cette manière par-fichier de clé.

Détails de l’attribut :

* `directoryPath` - Obligatoire. Spécifie un chemin d’accès pour rechercher les valeurs. Docker pour Windows secrets sont stockés dans le *C:\ProgramData\Docker\secrets* répertoire par défaut.
* `ignorePrefix` -Les fichiers qui commencent par ce préfixe sont exclus. Valeur par défaut est « ignorer ».
* `keyDelimiter` -Valeur par défaut est `null`. Si spécifié, le Générateur de configuration traverse plusieurs niveaux du répertoire, la création de noms de clés avec ce séparateur. Si cette valeur est `null`, le Générateur de configuration recherche uniquement au niveau supérieur du répertoire.
* `optional` -Valeur par défaut est `false`. Spécifie si le Générateur de configuration doit provoquer des erreurs si le répertoire source n’existe pas.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Jamais magasin de mots de passe, chaînes de connexion sensibles ou autres données sensibles dans le code source. Aucun secret de production ne doit pas être utilisé pour le développement ou de test.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Projets .NET core utilisent fréquemment des fichiers JSON pour la configuration. Le [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder permet aux fichiers de .NET Core JSON à utiliser dans le .NET Framework. Ce générateur de configuration est fournit un mappage de base à partir d’une source de clé-valeur plates en zones de clé/valeur spécifique de configuration de .NET Framework. Ce générateur de configuration est **pas** fournir pour les configurations hiérarchiques. Le fichier de sauvegarde JSON est similaire à un dictionnaire, pas un objet hiérarchiques complexes. Un fichier hiérarchique à plusieurs niveaux peut être utilisé. Ce fournisseur `flatten`la profondeur en ajoutant le nom de propriété à chaque niveau à l’aide de s `:` comme délimiteur.

Détails de l’attribut :

* `jsonFile` - Obligatoire. Spécifie le fichier JSON qui lit. Le `~` caractère peut être utilisé au début pour faire référence à la racine de l’application.
* `optional` -Booléenne, valeur par défaut est `true`. Empêche la levée d’exceptions si le fichier JSON est introuvable.
* `jsonMode` - `[Flat|Sectional]`. `Flat` est la valeur par défaut. Lorsque `jsonMode` est `Flat`, le fichier JSON est une source unique clé-valeur plates. Le `EnvironmentConfigBuilder` et `AzureKeyVaultConfigBuilder` sont également des sources de clé/valeur plat unique. Lorsque le `SimpleJsonConfigBuilder` est configuré dans `Sectional` mode :

  * Le fichier JSON est conceptuellement divisé simplement au niveau supérieur en plusieurs dictionnaires.
  * Chacun des dictionnaires est uniquement appliquée à la section de configuration qui correspond au nom de propriété de niveau supérieur qui leur sont attaché. Exemple :

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implémentation d’un générateur de configuration clé-valeur personnalisées

Si les générateurs de configuration ne répondent pas à vos besoins, vous pouvez écrire un personnalisé. Le `KeyValueConfigBuilder` classe de base gère les modes de substitution et la plupart des problèmes de préfixe. Un projet d’implémentation ont seulement besoin :

* Hériter de la classe de base et implémenter une source de base de paires clé/valeur via la `GetValue` et `GetAllValues`:
* Ajouter le [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) au projet.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

Le `KeyValueConfigBuilder` classe de base fournit une grande partie du comportement professionnel et cohérent entre la clé/valeur générateurs de configuration.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Dépôt GitHub de générateurs de configuration](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Authentification de service à service dans Azure Key Vault à l’aide de .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
