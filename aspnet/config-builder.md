---
uid: config-builder
title: Générateurs de configuration pour ASP.NET
author: rick-anderson
description: Découvrez comment obtenir des données de configuration à partir de sources autres que des valeurs de web.config à partir de sources externes.
ms.author: riande
ms.date: 7/17/2020
msc.type: content
ms.openlocfilehash: 1f95efcceb2ecf33fece12174cecf65cd8b27675
ms.sourcegitcommit: 000cbcd1de66fe8a766f203ef2d6f009e4435f53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2020
ms.locfileid: "86424796"
---
# <a name="configuration-builders-for-aspnet"></a>Générateurs de configuration pour ASP.NET

Par [Stephen Molloy](https://github.com/StephenMolloy) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Les générateurs de configuration fournissent un mécanisme moderne et agile permettant aux applications ASP.NET d’obtenir des valeurs de configuration à partir de sources externes.

Générateurs de configuration :

* Sont disponibles dans .NET Framework 4.7.1 et versions ultérieures.
* Fournir un mécanisme flexible pour la lecture des valeurs de configuration.
* Traitez certains des besoins de base des applications lorsqu’ils se déplacent dans un environnement de conteneur et de Cloud.
* Peut être utilisé pour améliorer la protection des données de configuration en dessinant à partir de sources précédemment indisponibles (par exemple, des Azure Key Vault et des variables d’environnement) dans le système de configuration .NET.

## <a name="keyvalue-configuration-builders"></a>Générateurs de configuration de clé/valeur

Un scénario courant qui peut être géré par les générateurs de configuration consiste à fournir un mécanisme de remplacement de clé/valeur de base pour les sections de configuration qui suivent un modèle clé/valeur. Le concept de .NET Framework de ConfigurationBuilders n’est pas limité aux sections ou modèles de configuration spécifiques. Toutefois, un grand nombre des générateurs de configuration dans `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) fonctionnent dans le modèle clé/valeur.

## <a name="keyvalue-configuration-builders-settings"></a>Paramètres des générateurs de configuration de clé/valeur

Les paramètres suivants s’appliquent à tous les générateurs de configuration de clé/valeur dans `Microsoft.Configuration.ConfigurationBuilders` .

### <a name="mode"></a>Mode

Les générateurs de configuration utilisent une source externe d’informations de clé/valeur pour remplir les éléments clé/valeur sélectionnés du système de configuration. Plus précisément, `<appSettings/>` les `<connectionStrings/>` sections et reçoivent un traitement spécial des générateurs de configuration. Les générateurs fonctionnent en trois modes :

* `Strict`: Mode par défaut. Dans ce mode, le générateur de configuration fonctionne uniquement sur des sections de configuration de clé/valeur bien connues. `Strict`mode énumère chaque clé dans la section. Si une clé correspondante est trouvée dans la source externe :

   * Les générateurs de configuration remplacent la valeur de la section de configuration obtenue par la valeur de la source externe.
* `Greedy`-Ce mode est étroitement lié au `Strict` mode. Plutôt que d’être limitées aux clés qui existent déjà dans la configuration d’origine :

  * Les générateurs de configuration ajoutent toutes les paires clé/valeur de la source externe dans la section de configuration résultante.

* `Expand`-Opère sur le fichier XML brut avant qu’il ne soit analysé dans un objet de section de configuration. Il peut être considéré comme une expansion des jetons dans une chaîne. Toute partie de la chaîne XML brute qui correspond au modèle `${token}` est candidate à l’expansion de jeton. Si aucune valeur correspondante n’est trouvée dans la source externe, le jeton n’est pas modifié. Les générateurs dans ce mode ne sont pas limités `<appSettings/>` aux `<connectionStrings/>` sections et.

Le balisage suivant de *web.config* active [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) en `Strict` mode :

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Le code suivant lit le `<appSettings/>` et le `<connectionStrings/>` affiché dans le fichier *web.config* précédent :

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Le code précédent définit les valeurs de propriété sur :

* Valeurs dans le fichier *web.config* si les clés ne sont pas définies dans les variables d’environnement.
* Valeurs de la variable d’environnement, si elles sont définies.

Par exemple, `ServiceID` contiendra :

* « ServiceID value from web.config », si la variable d’environnement `ServiceID` n’est pas définie.
* Valeur de la `ServiceID` variable d’environnement, si elle est définie.

L’illustration suivante montre les `<appSettings/>` clés/valeurs du jeu de fichiers *web.config* précédent dans l’éditeur d’environnement :

![éditeur d’environnement](config-builder/static/env.png)

Remarque : vous devrez peut-être quitter et redémarrer Visual Studio pour voir les modifications apportées aux variables d’environnement.

### <a name="prefix-handling"></a>Gestion des préfixes

Les préfixes de clé peuvent simplifier la définition des clés, car :

* La configuration .NET Framework est complexe et imbriquée.
* Les sources de clé/valeur externes sont généralement de base et plates par nature. Par exemple, les variables d’environnement ne sont pas imbriquées.

Utilisez l’une des approches suivantes pour injecter `<appSettings/>` et `<connectionStrings/>` dans la configuration via des variables d’environnement :

* Avec le `EnvironmentConfigBuilder` en mode par défaut `Strict` et les noms de clé appropriés dans le fichier de configuration. Le code et le balisage précédents adoptent cette approche. À l’aide de cette approche, vous **ne pouvez pas** avoir des clés portant le même nom dans `<appSettings/>` et `<connectionStrings/>` .
* Utilisez deux `EnvironmentConfigBuilder` s en `Greedy` mode avec des préfixes distincts et `stripPrefix` . Avec cette approche, l’application peut lire `<appSettings/>` et `<connectionStrings/>` sans avoir besoin de mettre à jour le fichier de configuration. La section suivante, [stripPrefix](#stripprefix), montre comment procéder.
* Utilisez deux `EnvironmentConfigBuilder` s en `Greedy` mode avec des préfixes distincts. Avec cette approche, vous ne pouvez pas avoir de noms de clé en double, car les noms de clé doivent différer par le préfixe.  Par exemple :

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Avec la balise précédente, la même source de clé/valeur plate peut être utilisée pour remplir la configuration de deux sections différentes.

L’illustration suivante montre les `<appSettings/>` `<connectionStrings/>` clés et/ou valeurs de l’ensemble de fichiers *web.config* précédent dans l’éditeur d’environnement :

![éditeur d’environnement](config-builder/static/prefix.png)

Le code suivant lit les `<appSettings/>` `<connectionStrings/>` clés et/ou valeurs contenues dans le fichier *web.config* précédent :

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Le code précédent définit les valeurs de propriété sur :

* Valeurs dans le fichier *web.config* si les clés ne sont pas définies dans les variables d’environnement.
* Valeurs de la variable d’environnement, si elles sont définies.

Par exemple, à l’aide du fichier de *web.config* précédent, des clés/valeurs dans l’image précédente de l’éditeur d’environnement et du code précédent, les valeurs suivantes sont définies :

|  Clé              | Valeur |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID à partir de variables env|
|    AppSetting_default            | Valeur AppSetting_default de env |
|       ConnStr_default         | ConnStr_default Val de env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: valeur booléenne, la valeur par défaut est `false` . 

Le balisage XML précédent sépare les paramètres d’application des chaînes de connexion, mais requiert toutes les clés dans le fichier *web.config* pour utiliser le préfixe spécifié. Par exemple, le préfixe `AppSetting` doit être ajouté à la `ServiceID` clé (« AppSetting_ServiceID »). Avec `stripPrefix` , le préfixe n’est pas utilisé dans le fichier *web.config* . Le préfixe est requis dans la source du générateur de configuration (par exemple, dans l’environnement). Nous pensons que la plupart des développeurs utiliseront `stripPrefix` .

En général, les applications suppriment le préfixe. Le *web.config* suivant supprime le préfixe :

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

Dans le fichier *web.config* précédent, la `default` clé se trouve à la fois dans `<appSettings/>` et `<connectionStrings/>` .

L’illustration suivante montre les `<appSettings/>` `<connectionStrings/>` clés et/ou valeurs de l’ensemble de fichiers *web.config* précédent dans l’éditeur d’environnement :

![éditeur d’environnement](config-builder/static/prefix.png)

Le code suivant lit les `<appSettings/>` `<connectionStrings/>` clés et/ou valeurs contenues dans le fichier *web.config* précédent :

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Le code précédent définit les valeurs de propriété sur :

* Valeurs dans le fichier *web.config* si les clés ne sont pas définies dans les variables d’environnement.
* Valeurs de la variable d’environnement, si elles sont définies.

Par exemple, à l’aide du fichier de *web.config* précédent, des clés/valeurs dans l’image précédente de l’éditeur d’environnement et du code précédent, les valeurs suivantes sont définies :

|  Clé              | Valeur |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID à partir de variables env|
|    default            | Valeur AppSetting_default de env |
|    default         | ConnStr_default Val de env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Chaîne, la valeur par défaut est`@"\$\{(\w+)\}"`

Le `Expand` comportement des générateurs recherche les jetons qui ressemblent dans le XML brut `${token}` . La recherche s’effectue avec l’expression régulière par défaut `@"\$\{(\w+)\}"` . Le jeu de caractères correspondant `\w` est plus strict que XML et de nombreuses sources de configuration le permettent. À utiliser `tokenPattern` lorsque plus de caractères que `@"\$\{(\w+)\}"` ne sont requis dans le nom du jeton.

`tokenPattern`Chaîne

* Permet aux développeurs de modifier l’expression régulière utilisée pour la correspondance des jetons.
* Aucune validation n’est effectuée pour s’assurer qu’il s’agit d’une expression régulière bien formée et non dangereuse.
* Il doit contenir un groupe de capture. L’expression régulière entière doit correspondre à l’intégralité du jeton. La première capture doit être le nom du jeton à rechercher dans la source de configuration.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Générateurs de configuration dans Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Est le plus simple des générateurs de configuration.
* Lit les valeurs de l’environnement.
* N’a pas d’options de configuration supplémentaires.
* La `name` valeur de l’attribut est arbitraire.

**Remarque :** Dans un environnement de conteneur Windows, les variables définies au moment de l’exécution sont injectées uniquement dans l’environnement de processus EntryPoint. Les applications qui s’exécutent en tant que service ou processus sans point d’entrée ne récupèrent pas ces variables, sauf si elles sont injectées par le biais d’un mécanisme dans le conteneur. Pour [IIS](https://github.com/Microsoft/iis-docker/pull/41) / les conteneurs basés sur IIS[ASP.net](https://github.com/Microsoft/aspnet-docker), la version actuelle de [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) gère cela dans *DefaultAppPool* uniquement. D’autres variantes de conteneur basées sur Windows peuvent avoir besoin de développer leur propre mécanisme d’injection pour les processus non EntryPoint.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Ne stockez jamais les mots de passe, les chaînes de connexion sensibles ou d’autres données sensibles dans le code source. Les secrets de production ne doivent pas être utilisés à des fins de développement ou de test.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Ce générateur de configuration offre une fonctionnalité similaire à [ASP.net Core gestionnaire de secret](/aspnet/core/security/app-secrets).

Le [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) peut être utilisé dans .NET Framework projets, mais un fichier de secrets doit être spécifié. Vous pouvez également définir la `UserSecretsId` propriété dans le fichier projet et créer le fichier de secrets bruts à l’emplacement correct pour la lecture. Pour conserver les dépendances externes de votre projet, le fichier de secret est au format XML. La mise en forme XML est un détail d’implémentation et le format ne doit pas être basé sur. Si vous devez partager un *secrets.jssur* un fichier avec des projets .net Core, envisagez d’utiliser [SimpleJsonConfigBuilder](#simplejsonconfigbuilder). Le `SimpleJsonConfigBuilder` format pour .net Core doit également être considéré comme un détail d’implémentation susceptible de changer.

Attributs de configuration pour `UserSecretsConfigBuilder` :

* `userSecretsId`-Il s’agit de la méthode recommandée pour identifier un fichier de secrets XML. Il fonctionne de la même façon que .NET Core, qui utilise une `UserSecretsId` propriété de projet pour stocker cet identificateur. La chaîne doit être unique, il n’est pas nécessaire qu’il s’agit d’un GUID. Avec cet attribut, `UserSecretsConfigBuilder` recherchez dans un emplacement local connu ( `%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml` ) un fichier de secrets appartenant à cet identificateur.
* `userSecretsFile`-Attribut facultatif spécifiant le fichier contenant les secrets. Le `~` caractère peut être utilisé au début pour référencer la racine de l’application. Cet attribut ou l' `userSecretsId` attribut est requis. Si les deux sont spécifiés, `userSecretsFile` est prioritaire.
* `optional`: valeur booléenne, valeur par défaut `true` -empêche une exception si le fichier de secrets est introuvable. 
* La `name` valeur de l’attribut est arbitraire.

Le format du fichier de secrets est le suivant :

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

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lit les valeurs stockées dans le [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName`est requis (nom du coffre ou URI du coffre). Les autres attributs permettent de contrôler le coffre auquel se connecter, mais ne sont nécessaires que si l’application ne s’exécute pas dans un environnement qui fonctionne avec `Microsoft.Azure.Services.AppAuthentication` . La bibliothèque d’authentification des services Azure est utilisée pour récupérer automatiquement les informations de connexion à partir de l’environnement d’exécution, si possible. Vous pouvez remplacer automatiquement les informations de connexion en fournissant une chaîne de connexion.

* `vaultName`-Obligatoire si `uri` n’est pas fourni. Spécifie le nom du coffre dans votre abonnement Azure à partir duquel lire les paires clé/valeur.
* `connectionString`-Chaîne de connexion utilisable par [le paramètre azureservicetokenprovider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri`-Se connecte à d’autres fournisseurs de Key Vault avec la `uri` valeur spécifiée. S’il n’est pas spécifié, Azure ( `vaultName` ) est le fournisseur du coffre.
* `version`-Azure Key Vault fournit une fonctionnalité de contrôle de version pour les secrets. Si `version` est spécifié, le générateur récupère uniquement les secrets correspondant à cette version.
* `preloadSecretNames`-Par défaut, ce générateur interroge **tous les** noms de clé dans le coffre de clés lors de son initialisation. Pour empêcher la lecture de toutes les valeurs de clés, affectez à cet attribut la valeur `false` . La définition de cette option pour `false` lire les secrets un à la fois. La lecture d’un secret à la fois peut être utile si le coffre autorise un accès « obtenir », mais pas un accès « liste ». **Remarque :** Lors de l’utilisation `Greedy` du mode, `preloadSecretNames` doit avoir `true` la valeur (valeur par défaut).

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

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) est un générateur de configuration de base qui utilise les fichiers d’un répertoire comme source de valeurs. Le nom d’un fichier est la clé et le contenu est la valeur. Ce générateur de configuration peut être utile lors de l’exécution dans un environnement de conteneurs orchestrés. Les systèmes tels que la station d’accueil essaim et Kubernetes fournissent `secrets` à leurs conteneurs Windows orchestrés dans cette clé par fichier.

Détails de l’attribut :

* `directoryPath` - Obligatoire. Spécifie un chemin d’accès pour rechercher des valeurs. Docker pour Windows les secrets sont stockés dans le répertoire *C:\ProgramData\Docker\secrets* par défaut.
* `ignorePrefix`-Les fichiers qui commencent par ce préfixe sont exclus. La valeur par défaut est « ignore ».
* `keyDelimiter`-La valeur par défaut est `null` . S’il est spécifié, le générateur de configuration parcourt plusieurs niveaux de l’annuaire, en générant des noms de clés avec ce délimiteur. Si cette valeur est `null` , le générateur de configuration recherche uniquement le niveau supérieur de l’annuaire.
* `optional`-La valeur par défaut est `false` . Spécifie si le générateur de configuration doit provoquer des erreurs si le répertoire source n’existe pas.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Ne stockez jamais les mots de passe, les chaînes de connexion sensibles ou d’autres données sensibles dans le code source. Les secrets de production ne doivent pas être utilisés à des fins de développement ou de test.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Les projets .NET Core utilisent fréquemment des fichiers JSON pour la configuration. [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) Builder permet d’utiliser les fichiers JSON .net core dans le .NET Framework. Ce générateur de configuration fournit un mappage de base à partir d’une source de clé/valeur plate dans des zones clé/valeur spécifiques de .NET Framework Configuration. Ce générateur de configuration ne fournit **pas** de configurations hiérarchiques. Le fichier de stockage JSON est similaire à un dictionnaire, et non à un objet hiérarchique complexe. Un fichier hiérarchique à plusieurs niveaux peut être utilisé. Ce fournisseur est `flatten` la profondeur en ajoutant le nom de la propriété à chaque niveau à l’aide de `:` comme délimiteur.

Détails de l’attribut :

* `jsonFile` - Obligatoire. Spécifie le fichier JSON à partir duquel effectuer la lecture. Le `~` caractère peut être utilisé au début pour référencer la racine de l’application.
* `optional`-Boolean, la valeur par défaut est `true` . Empêche de lever des exceptions si le fichier JSON est introuvable.
* `jsonMode` - `[Flat|Sectional]`. `Flat` est la valeur par défaut. Lorsque `jsonMode` a `Flat` la valeur, le fichier JSON est une source de clé/valeur unique. Les `EnvironmentConfigBuilder` et `AzureKeyVaultConfigBuilder` sont également des sources de clé/valeur à plat unique. Lorsque le `SimpleJsonConfigBuilder` est configuré en `Sectional` mode :

  * Le fichier JSON est divisé de façon conceptuelle juste au niveau supérieur en plusieurs dictionnaires.
  * Chacun des dictionnaires est appliqué uniquement à la section de configuration qui correspond au nom de propriété de niveau supérieur associé. Par exemple :

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

## <a name="configuration-builders-order"></a>Ordre des générateurs de configurations

Consultez [ordre d’exécution de ConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/master/README.md#configurationbuilders-order-of-execution) dans le référentiel GitHub [ASPNET/MicrosoftConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) .

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implémentation d’un générateur de configuration de clé/valeur personnalisé

Si les générateurs de configuration ne répondent pas à vos besoins, vous pouvez en écrire un personnalisé. La `KeyValueConfigBuilder` classe de base gère les modes de substitution et la plupart des problèmes de préfixe. Un projet d’implémentation a besoin uniquement des éléments suivants :

* Héritez de la classe de base et implémentez une source de base de paires clé/valeur à l’aide de `GetValue` et `GetAllValues` :
* Ajoutez le [Microsoft.Configuration.ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) au projet.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

La `KeyValueConfigBuilder` classe de base fournit une grande partie du travail et un comportement cohérent entre les générateurs de configuration de clé/valeur.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Référentiel GitHub générateurs de configuration](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Authentification de service à service auprès d’Azure Key Vault à l’aide de .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
