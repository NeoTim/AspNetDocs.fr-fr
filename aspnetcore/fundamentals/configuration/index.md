---
title: Configuration dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser l’API de configuration pour configurer une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/25/2019
uid: fundamentals/configuration/index
---
# <a name="configuration-in-aspnet-core"></a>Configuration dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

La configuration d’application dans ASP.NET Core est basée sur des paires clé-valeur établies par les *fournisseurs de configuration*. Les fournisseurs de configuration lisent les données de configuration dans les paires clé-valeur à partir de diverses sources de configuration :

::: moniker range=">= aspnetcore-2.1"

* Azure Key Vault
* Arguments de ligne de commande
* Fournisseurs personnalisés (installés ou créés)
* Fichiers de répertoire
* Variables d’environnement
* Objets .NET en mémoire
* Fichiers de paramètres

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* Azure Key Vault
* Arguments de ligne de commande
* Fournisseurs personnalisés (installés ou créés)
* Variables d’environnement
* Objets .NET en mémoire
* Fichiers de paramètres

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* Arguments de ligne de commande
* Fournisseurs personnalisés (installés ou créés)
* Variables d’environnement
* Objets .NET en mémoire
* Fichiers de paramètres

::: moniker-end

Le *modèle d’options* est une extension des concepts de configuration décrits dans cette rubrique. Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés. Pour plus d’informations sur l’utilisation du modèle d’options, consultez <xref:fundamentals/configuration/options>.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

Ces trois packages sont inclus dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Ces trois packages sont inclus dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

::: moniker-end

## <a name="host-vs-app-configuration"></a>Configuration de l’hôte ou configuration de l’application

Avant que l’application ne soit configurée et démarrée, un *hôte* est configuré et lancé. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications. L’application et l’hôte sont configurés à l’aide des fournisseurs de configuration décrits dans cette rubrique. Les paires clé-valeur de la configuration de l’hôte font partie de la configuration générale de l’application. Pour plus d’informations sur l’utilisation des fournisseurs de configuration lors de la génération de l’hôte et l’impact des sources de configuration sur la configuration de l’hôte, consultez [L’hôte](xref:fundamentals/index#host).

## <a name="default-configuration"></a>Configuration par défaut

Les applications web basées sur les modèles ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) appellent <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pendant la création d’un hôte. `CreateDefaultBuilder` fournit la configuration par défaut de l’application.

* La configuration de l’hôte est fournie à partir des éléments suivants :
  * Des variables d’environnement préfixées avec `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`) à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).
  * Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).
* La configuration de l’application est fournie à partir des éléments suivants (dans l’ordre) :
  * *appSettings.JSON* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).
  * *appsettings.{Environment}.json* à l’aide du [Fournisseur de configuration de fichier](#file-configuration-provider).
  * L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.
  * Des variables d’environnement à l’aide du [Fournisseur de configuration des variables d’environnement](#environment-variables-configuration-provider).
  * Des arguments de ligne de commande à l’aide du [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider).

Les fournisseurs de configuration sont décrits plus loin dans cette rubrique. Pour plus d’informations sur l’hôte et `CreateDefaultBuilder`, consultez <xref:fundamentals/host/web-host#set-up-a-host>.

## <a name="security"></a>Sécurité

Adoptez les meilleures pratiques suivantes :

* Ne stockez jamais des mots de passe ou d’autres données sensibles dans le code du fournisseur de configuration ou dans les fichiers de configuration en texte clair.
* N’utilisez aucun secret de production dans les environnements de développement ou de test.
* Spécifiez les secrets en dehors du projet afin qu’ils ne puissent pas être validés par inadvertance dans un référentiel de code source.

En savoir plus sur [l’utilisation de plusieurs environnements](xref:fundamentals/environments) et la gestion du [stockage sécurisé des secrets d’application dans le développement avec Secret Manager](xref:security/app-secrets) (contient des conseils sur l’utilisation des variables d’environnement pour stocker les données sensibles). Secret Manager utilise le fournisseur de configuration de fichier pour stocker les secrets utilisateur dans un fichier JSON sur le système local. Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) constitue une option pour le stockage sécurisé des secrets d’application. Pour plus d'informations, consultez <xref:security/key-vault-configuration>.

## <a name="hierarchical-configuration-data"></a>Données de configuration hiérarchiques

L’API Configuration est capable de maintenir des données de configuration hiérarchiques en aplanissant les données hiérarchiques à l’aide d’un délimiteur dans les clés de configuration.

Dans le fichier JSON suivant, quatre clés existent dans une structure hiérarchique à deux sections :

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

Lorsque le fichier est lu dans la configuration, des clés uniques sont créées pour gérer la structure des données hiérarchiques d’origine de la source de configuration. Les sections et les clés sont aplanies à l’aide d’un signe deux-points (`:`) pour conserver la structure d’origine :

* section0:key0
* section0:key1
* section1:key0
* section1:key1

Les méthodes <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> et <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sont disponibles pour isoler les sections et les enfants d’une section dans les données de configuration. Ces méthodes sont décrites plus loin dans [GetSection, GetChildren et Exists](#getsection-getchildren-and-exists). `GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="conventions"></a>Conventions

Au démarrage de l’application, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés.

Les fournisseurs de configuration de fichier peuvent recharger la configuration lorsqu’un fichier de paramètres sous-jacent est modifié après le démarrage de l’application. Le fournisseur de configuration de fichier est décrit plus loin dans cette rubrique.

<xref:Microsoft.Extensions.Configuration.IConfiguration> est disponible dans le conteneur [Dependency Injection (DI)](xref:fundamentals/dependency-injection) de l’application. Les fournisseurs de configuration ne peuvent pas utiliser le DI, car celui-ci n’est pas disponible lorsque les fournisseurs sont configurés par l’hôte.

Les clés de configuration adoptent les conventions suivantes :

* Les clés ne respectent pas la casse. Par exemple, `ConnectionString` et `connectionstring` sont traités en tant que clés équivalentes.
* Si une valeur pour la même clé est définie par des fournisseurs de configuration identiques ou différents, la valeur utilisée est la dernière valeur définie sur la clé.
* Clés hiérarchiques
  * Dans l’API Configuration, un séparateur sous forme de signe deux-points (`:`) fonctionne sur toutes les plateformes.
  * Dans les variables d’environnement, un séparateur sous forme de signe deux-points peut ne pas fonctionner sur toutes les plateformes. Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et converti en signe deux-points.
  * Dans Azure Key Vault, les clés hiérarchiques utilisent `--` (deux tirets) comme séparateur. Vous devez fournir du code pour remplacer les tirets par un signe deux-points lorsque les secrets sont chargés dans la configuration de l’application.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration. La liaison de tableau est décrite dans la section [Lier un tableau à une classe](#bind-an-array-to-a-class).

Les valeurs de configuration adoptent les conventions suivantes :

* Les valeurs sont des chaînes.
* Les valeurs NULL ne peuvent pas être stockées dans la configuration ou liées à des objets.

## <a name="providers"></a>Fournisseurs

Le tableau suivant présente les fournisseurs de configuration disponibles pour les applications ASP.NET Core.

::: moniker range=">= aspnetcore-2.1"

| Fournisseur | Fournit la configuration à partir de&hellip; |
| -------- | ----------------------------------- |
| [Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*) | Azure Key Vault |
| [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider) | Paramètres de ligne de commande |
| [Fournisseur de configuration personnalisé](#custom-configuration-provider) | Source personnalisée |
| [Fournisseur de configuration de variables d’environnement](#environment-variables-configuration-provider) | Variables d’environnement |
| [Fournisseur de configuration de fichier](#file-configuration-provider) | Fichiers (INI, JSON, XML) |
| [Fournisseur de configuration clé par fichier](#key-per-file-configuration-provider) | Fichiers de répertoire |
| [Fournisseur de configuration de mémoire](#memory-configuration-provider) | Collections en mémoire |
| [Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*) | Fichier dans le répertoire de profil utilisateur |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| Fournisseur | Fournit la configuration à partir de&hellip; |
| -------- | ----------------------------------- |
| [Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration) (rubrique *Sécurité*) | Azure Key Vault |
| [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider) | Paramètres de ligne de commande |
| [Fournisseur de configuration personnalisé](#custom-configuration-provider) | Source personnalisée |
| [Fournisseur de configuration de variables d’environnement](#environment-variables-configuration-provider) | Variables d’environnement |
| [Fournisseur de configuration de fichier](#file-configuration-provider) | Fichiers (INI, JSON, XML) |
| [Fournisseur de configuration de mémoire](#memory-configuration-provider) | Collections en mémoire |
| [Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*) | Fichier dans le répertoire de profil utilisateur |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| Fournisseur | Fournit la configuration à partir de&hellip; |
| -------- | ----------------------------------- |
| [Fournisseur de configuration de ligne de commande](#command-line-configuration-provider) | Paramètres de ligne de commande |
| [Fournisseur de configuration personnalisé](#custom-configuration-provider) | Source personnalisée |
| [Fournisseur de configuration de variables d’environnement](#environment-variables-configuration-provider) | Variables d’environnement |
| [Fournisseur de configuration de fichier](#file-configuration-provider) | Fichiers (INI, JSON, XML) |
| [Fournisseur de configuration de mémoire](#memory-configuration-provider) | Collections en mémoire |
| [Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (rubrique *Sécurité*) | Fichier dans le répertoire de profil utilisateur |

::: moniker-end

Au démarrage, les sources de configuration sont lues dans l’ordre où leurs fournisseurs de configuration sont spécifiés. Les fournisseurs de configuration décrits dans cette rubrique sont décrits dans l’ordre alphabétique, et non dans l’ordre où votre code peut les organiser. Organisez les fournisseurs de configuration dans votre code en fonction de vos priorités pour les sources de configuration sous-jacentes.

Une séquence type des fournisseurs de configuration est la suivante :

1. Fichiers (*appsettings.json*, *appsettings.{Environment}.json*, où `{Environment}` est l'environnement d’hébergement actuel de l'application)
1. [Azure Key Vault](xref:security/key-vault-configuration)
1. [Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement uniquement)
1. Variables d’environnement
1. Arguments de ligne de commande

Une pratique courante consiste à placer le Fournisseur de configuration de ligne de commande en dernier dans une série de fournisseurs pour permettre aux arguments de ligne de commande de remplacer la configuration définie par les autres fournisseurs.

::: moniker range=">= aspnetcore-2.0"

Cette séquence de fournisseurs est mise en place lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Cette séquence de fournisseurs peut être créée pour l’application (pas l’hôte) avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> et un appel à sa méthode <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> dans `Startup` :

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

Dans l’exemple précédent, le nom de l’environnement (`env.EnvironmentName`) et le nom de l’assembly d’application (`env.ApplicationName`) sont fournis par <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>. Pour plus d'informations, consultez <xref:fundamentals/environments>. Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quand vous créez l’hôte pour spécifier les fournisseurs de configuration de l’application en plus de ceux ajoutés automatiquement par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> :

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a>Fournisseur de configuration de ligne de commande

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> charge la configuration à partir des paires clé-valeur de l’argument de ligne de commande lors de l’exécution.

Pour activer la configuration en ligne de commande, la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> est appelée sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

::: moniker range=">= aspnetcore-2.0"

`AddCommandLine` est appelé automatiquement lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` charge également :

* Configuration facultative à partir d’*appsettings.json* et d’*appsettings.{Environment}.json*.
* [Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).
* Variables d'environnement.

`CreateDefaultBuilder` ajoute en dernier le Fournisseur de configuration de ligne de commande. Les arguments de ligne de commande passés lors de l’exécution remplacent la configuration définie par les autres fournisseurs.

`CreateDefaultBuilder` agit lors de la construction de l’hôte. Par conséquent, la configuration de ligne de commande activée par `CreateDefaultBuilder` peut affecter la façon dont l’hôte est configuré.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application.

`AddCommandLine` a déjà été appelé par `CreateDefaultBuilder`. Si vous devez fournir la configuration de l'application tout en étant capable de remplacer cette configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l'application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> et appelez `AddCommandLine` en dernier.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.

`AddCommandLine` a déjà été appelé par `CreateDefaultBuilder` lorsque `UseConfiguration` est appelé. Si vous devez fournir la configuration de l'application tout en étant capable de remplacer cette configuration par des arguments de ligne de commande, appelez les fournisseurs supplémentaires de l'application sur un `ConfigurationBuilder` et appelez `AddCommandLine` en dernier.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Pour activer la configuration en ligne de commande, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Appelez le fournisseur en dernier pour permettre aux arguments de ligne de commande passés au moment de l’exécution de remplacer la configuration définie par les autres fournisseurs de configuration.

Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Exemple**

::: moniker range=">= aspnetcore-2.0"

L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

L’exemple d’application 1.x appelle <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> sur un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

::: moniker-end

1. Ouvrez une invite de commandes dans le répertoire du projet.
1. Fournissez un argument de ligne de commande à la commande `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.
1. Une fois que l’application est en cours d’exécution, ouvrez un navigateur à l’application à l’adresse `http://localhost:5000`.
1. Notez que la sortie contient la paire clé-valeur pour l’argument de ligne de commande de configuration fourni à `dotnet run`.

### <a name="arguments"></a>Arguments

La valeur doit suivre un signe égal (`=`) ou la clé doit avoir un préfixe (`--` ou `/`) lorsque la valeur suit un espace. La valeur peut être null si un signe égal est utilisé (par exemple, `CommandLineKey=`).

| Préfixe de clé               | Exemple                                                |
| ------------------------ | ------------------------------------------------------ |
| Aucun préfixe                | `CommandLineKey1=value1`                               |
| Deux tirets (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Barre oblique (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

Dans la même commande, ne mélangez pas des paires clé-valeur de l’argument de ligne de commande qui utilisent un signe égal avec des paires clé-valeur qui utilisent un espace.

Exemples de commandes :

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Correspondances de commutateur

Les correspondances de commutateur permettent une logique de remplacement des noms de clés. Lorsque vous générez manuellement la configuration avec un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, vous pouvez fournir un dictionnaire des remplacements de commutateur à la méthode <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

Quand le dictionnaire de correspondances de commutateur est utilisé, il est vérifié afin de déterminer s’il contient une clé correspondant à celle fournie par un argument de ligne de commande. Si la clé de ligne de commande est trouvée dans le dictionnaire, la valeur du dictionnaire (le remplacement de la clé) est repassée pour définir la paire clé-valeur dans la configuration de l’application. Une correspondance de commutateur est nécessaire pour chaque clé de ligne de commande préfixée avec un tiret unique (`-`).

Règles des clés du dictionnaire de correspondances de commutateur :

* Les commutateurs doivent commencer par un tiret (`-`) ou un double tiret (`--`).
* Le dictionnaire de correspondances de commutateur ne doit pas contenir de clés en double.

::: moniker range=">= aspnetcore-2.1"

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

Comme indiqué dans l’exemple précédent, l’appel à `CreateDefaultBuilder` ne doit pas passer des arguments lorsque des correspondances de commutateur sont utilisées. L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`. Si les arguments incluent un commutateur mappé et sont passés à `CreateDefaultBuilder`, son fournisseur `AddCommandLine` ne parvient pas à s’initialiser avec un <xref:System.FormatException>. La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de mappage de commutateur.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Comme indiqué dans l’exemple précédent, l’appel à `CreateDefaultBuilder` ne doit pas passer des arguments lorsque des correspondances de commutateur sont utilisées. L’appel `AddCommandLine` de la méthode `CreateDefaultBuilder` n’inclut pas de commutateurs mappés et il n’existe aucun moyen de transmettre le dictionnaire de correspondance de commutateur à `CreateDefaultBuilder`. Si les arguments incluent un commutateur mappé et sont passés à `CreateDefaultBuilder`, son fournisseur `AddCommandLine` ne parvient pas à s’initialiser avec un <xref:System.FormatException>. La solution ne consiste pas à transmettre les arguments à `CreateDefaultBuilder`, mais plutôt à permettre à la méthode `AddCommandLine` de la méthode `ConfigurationBuilder` de traiter les arguments et le dictionnaire de mappage de commutateur.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

Une fois le dictionnaire de correspondances de commutateur créé, il contient les données affichées dans le tableau suivant.

| Touche       | Value             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Si les clés mappées au commutateur sont utilisées lors du démarrage de l’application, la configuration reçoit la valeur de configuration sur la clé fournie par le dictionnaire :

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

Après avoir exécuté la commande précédente, la configuration contient les valeurs indiquées dans le tableau suivant.

| Touche               | Value    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>Fournisseur de configuration de variables d’environnement

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> charge la configuration à partir des paires clé-valeur de la variable d’environnement lors de l’exécution.

Pour activer la configuration des variables d’environnement, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Lors de l’utilisation de clés hiérarchiques dans les variables d’environnement, un séparateur sous forme de signe deux-points (`:`) peut ne pas fonctionner sur toutes les plateformes. Un trait de soulignement double (`__`) est pris en charge par toutes les plateformes et remplacé par un signe deux-points.

[Azure App Service](https://azure.microsoft.com/services/app-service/) vous permet de définir des variables d’environnement dans le portail Azure capables de remplacer la configuration d’application à l’aide du Fournisseur de configuration de variables d’environnement. Pour plus d’informations, consultez les [applications Azure : Remplacer la configuration de l’application à l’aide du Portail Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

::: moniker range=">= aspnetcore-2.0"

`AddEnvironmentVariables` est appelé automatiquement pour les variables d’environnement précédées de `ASPNETCORE_` à l’initialisation d’un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>. Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` charge également :

* Configuration de l’application à partir de variables d’environnement sans préfixe en appelant `AddEnvironmentVariables` sans préfixe.
* Configuration facultative à partir d’*appsettings.json* et d’*appsettings.{Environment}.json*.
* [Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).
* Arguments de ligne de commande

Le fournisseur de configuration de variables d’environnement est appelé une fois que la configuration est établie à partir des secrets utilisateur et des fichiers *appsettings*. Le fait d’appeler le fournisseur ainsi permet de lire les variables d’environnement pendant l’exécution pour substituer la configuration définie par les secrets utilisateur et les fichiers *appsettings*.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application.

Si vous devez fournir la configuration de l'application à partir de variables d'environnement supplémentaires, appelez les fournisseurs supplémentaires de l'application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> et appelez `AddEnvironmentVariables` avec le préfixe.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Appelez la méthode d’extension `AddEnvironmentVariables` sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.

Si vous devez fournir la configuration de l'application à partir de variables d'environnement supplémentaires, appelez les fournisseurs supplémentaires de l'application dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> et appelez `AddEnvironmentVariables` avec le préfixe.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Exemple**

::: moniker range=">= aspnetcore-2.0"

L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut un appel à `AddEnvironmentVariables`.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

L’exemple d’application 1.x appelle `AddEnvironmentVariables` sur un `ConfigurationBuilder`.

::: moniker-end

1. Exécutez l’exemple d’application. Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.
1. Notez que la sortie contient la paire clé-valeur pour la variable d’environnement `ENVIRONMENT`. La valeur reflète l’environnement dans lequel l’application est en cours d’exécution, en général `Development` lors de l’exécution locale.

Pour que la liste des variables d’environnement restituée par l’application soit courte, l’application filtre les variables d’environnement en fonction de celles qui commencent par :

* ASPNETCORE_
* urls
* Journalisation
* ENVIRONMENT
* contentRoot
* AllowedHosts
* applicationName
* CommandLine

::: moniker range=">= aspnetcore-2.0"

Si vous souhaitez exposer toutes les variables d’environnement disponibles pour l’application, remplacez `FilteredConfiguration` dans *Pages/Index.cshtml.cs* par ce qui suit :

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si vous souhaitez exposer toutes les variables d’environnement disponibles pour l’application, remplacez `FilteredConfiguration` dans *Controllers/HomeController.cs* par ce qui suit :

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Préfixes

Les variables d’environnement chargées dans la configuration de l’application sont filtrées lorsque vous fournissez un préfixe pour la méthode `AddEnvironmentVariables`. Par exemple, pour filtrer les variables d’environnement sur le préfixe `CUSTOM_`, fournissez le préfixe au fournisseur de configuration :

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

Le préfixe est supprimé lorsque les paires clé-valeur de la configuration sont créées.

::: moniker range=">= aspnetcore-2.0"

La méthode pratique statique `CreateDefaultBuilder` crée un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> pour établir l’hôte de l’application. Lorsque `WebHostBuilder` est créé, il trouve la configuration de son hôte dans les variables d’environnement avec le préfixe `ASPNETCORE_`.

::: moniker-end

**Préfixes des chaînes de connexion**

L’API Configuration possède des règles de traitement spéciales pour quatre variables d’environnement de chaîne de connexion impliquées dans la configuration des chaînes de connexion Azure pour l’environnement de l’application. Les variables d’environnement avec les préfixes indiqués dans le tableau sont chargées dans l’application si aucun préfixe n’est fourni à `AddEnvironmentVariables`.

| Préfixe de la chaîne de connexion | Fournisseur |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Fournisseur personnalisé |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Quand une variable d’environnement est découverte et chargée dans la configuration avec l’un des quatre préfixes indiqués dans le tableau :

* La clé de configuration est créée en supprimant le préfixe de la variable d’environnement et en ajoutant une section de clé de configuration (`ConnectionStrings`).
* Une nouvelle paire clé-valeur de configuration est créée qui représente le fournisseur de connexion de base de données (à l’exception de `CUSTOMCONNSTR_`, qui ne possède aucun fournisseur indiqué).

| Clé de variable d’environnement | Clé de configuration convertie | Entrée de configuration de fournisseur                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | Entrée de configuration non créée.                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | Clé : `ConnectionStrings:<KEY>_ProviderName` :<br>Valeur : `MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | Clé : `ConnectionStrings:<KEY>_ProviderName` :<br>Valeur : `System.Data.SqlClient`  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | Clé : `ConnectionStrings:<KEY>_ProviderName` :<br>Valeur : `System.Data.SqlClient`  |

## <a name="file-configuration-provider"></a>Fournisseur de configuration de fichier

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> est la classe de base pour charger la configuration à partir du système de fichiers. Les fournisseurs de configuration suivants sont dédiés à des types de fichiers spécifiques :

* [Fournisseur de configuration INI](#ini-configuration-provider)
* [Fournisseur de configuration JSON](#json-configuration-provider)
* [Fournisseur de configuration XML](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>Fournisseur de configuration INI

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier INI lors de l’exécution.

Pour activer la configuration du fichier INI, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Le signe deux-points peut servir de délimiteur de section dans la configuration d’un fichier INI.

Les surcharges permettent de spécifier :

* Si le fichier est facultatif.
* Si la configuration est rechargée quand le fichier est modifié.
* Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.

::: moniker range=">= aspnetcore-2.1"

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Lors de l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Exemple générique d’un fichier de configuration INI :

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

Le fichier de configuration précédent charge les clés suivantes avec `value` :

* section0:key0
* section0:key1
* section1:subsection:key
* section2:subsection0:key
* section2:subsection1:key

### <a name="json-configuration-provider"></a>Fournisseur de configuration JSON

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier JSON lors de l’exécution.

Pour activer la configuration du fichier JSON, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Les surcharges permettent de spécifier :

* Si le fichier est facultatif.
* Si la configuration est rechargée quand le fichier est modifié.
* Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.

::: moniker range=">= aspnetcore-2.0"

`AddJsonFile` est appelé automatiquement deux fois lorsque vous initialisez un nouveau <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. La méthode est appelée pour charger la configuration à partir de :

* *appSettings.JSON* &ndash; Ce fichier est lu en premier. La version de l’environnement du fichier peut remplacer les valeurs fournies par le fichier *appsettings.json*.
* *appsettings.{Environment}.json* &ndash; La version de l’environnement du fichier est chargée à partir du fichier [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Pour plus d’informations, consultez l’[hôte web : Configurer un hôte](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` charge également :

* Variables d'environnement.
* [Secrets utilisateur (Secret Manager)](xref:security/app-secrets) (dans l’environnement de développement).
* Arguments de ligne de commande

Le Fournisseur de configuration JSON est établi en premier. Par conséquent, les secrets utilisateur, les variables d’environnement et les arguments de ligne de commande remplacent la configuration définie par les fichiers *appsettings*.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application pour les fichiers autres qu’*appsettings.json* et *appsettings. {Environment} .json* :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Vous pouvez aussi appeler directement la méthode d’extension `AddJsonFile` sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Lors de l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

**Exemple**

::: moniker range=">= aspnetcore-2.0"

L’exemple d’application 2.x tire parti de la méthode pratique statique `CreateDefaultBuilder` pour générer l’hôte, qui inclut deux appels à `AddJsonFile`. La configuration est chargée à partir d’*appsettings.json* et d’*appsettings{Environment}.json*.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

L’exemple d’application 1.x appelle `AddJsonFile` deux fois sur un `ConfigurationBuilder`. La configuration est chargée à partir d’*appsettings.json* et d’*appsettings{Environment}.json*.

::: moniker-end

1. Exécutez l’exemple d’application. Ouvrez un navigateur vers l’application avec l’adresse `http://localhost:5000`.
1. Notez que la sortie contient des paires clé-valeur pour la configuration représentée dans le tableau en fonction de l’environnement. Les clés de configuration de la journalisation utilisent le signe deux-points (`:`) comme séparateur hiérarchique.

| Touche                        | Valeur de développement | Valeur de production |
| -------------------------- | :---------------: | :--------------: |
| Logging:LogLevel:System    | Information       | Information      |
| Logging:LogLevel:Microsoft | Information       | Information      |
| Logging:LogLevel:Default   | Débogage             | Error            |
| AllowedHosts               | *                 | *                |

### <a name="xml-configuration-provider"></a>Fournisseur de configuration XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> charge la configuration à partir des paires clé-valeur du fichier XML lors de l’exécution.

Pour activer la configuration du fichier XML, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Les surcharges permettent de spécifier :

* Si le fichier est facultatif.
* Si la configuration est rechargée quand le fichier est modifié.
* Le <xref:Microsoft.Extensions.FileProviders.IFileProvider> utilisé pour accéder au fichier.

Le nœud racine du fichier de configuration est ignoré lorsque les paires clé-valeur de la configuration sont créées. Ne spécifiez pas une définition de type de document (DTD) ou un espace de noms dans le fichier.

::: moniker range=">= aspnetcore-2.1"

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Lors de l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Les fichiers de configuration XML peuvent utiliser des noms d’éléments distincts pour les sections répétitives :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

Le fichier de configuration précédent charge les clés suivantes avec `value` :

* section0:key0
* section0:key1
* section1:key0
* section1:key1

Les éléments répétitifs qui utilisent le même nom d’élément fonctionnent si l’attribut `name` est utilisé pour distinguer les éléments :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

Le fichier de configuration précédent charge les clés suivantes avec `value` :

* section:section0:key:key0
* section:section0:key:key1
* section:section1:key:key0
* section:section1:key:key1

Les attributs peuvent être utilisés pour fournir des valeurs :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

Le fichier de configuration précédent charge les clés suivantes avec `value` :

* key:attribute
* section:key:attribute

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a>Fournisseur de configuration clé par fichier

Le <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> utilise les fichiers d’un répertoire en tant que paires clé-valeur de configuration. La clé est le nom de fichier. La valeur contient le contenu du fichier. Le Fournisseur de configuration Clé par fichier est utilisé dans les scénarios d’hébergement de Docker.

Pour activer la configuration clé par fichier, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. Le `directoryPath` aux fichiers doit être un chemin d’accès absolu.

Les surcharges permettent de spécifier :

* Un délégué `Action<KeyPerFileConfigurationSource>` qui configure la source.
* Si le répertoire est facultatif et le chemin d’accès au répertoire.

Le double trait de soulignement (`__`) est utilisé comme un délimiteur de clé de configuration dans les noms de fichiers. Par exemple, le nom de fichier `Logging__LogLevel__System` génère la clé de configuration `Logging:LogLevel:System`.

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

Le chemin de base est défini avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` est dans le package [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a>Fournisseur de configuration de mémoire

Le <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> utilise une collection en mémoire en tant que paires clé-valeur de configuration.

Pour activer la configuration de la collection en mémoire, appelez la méthode d’extension <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> sur une instance de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Le fournisseur de configuration peut être initialisé avec un `IEnumerable<KeyValuePair<String,String>>`.

::: moniker range=">= aspnetcore-2.1"

Appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> lors de la création de l’hôte pour spécifier la configuration de l’application :

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Lors de l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

        var config = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Lorsque vous créez un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directement, appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> avec la configuration :

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Appliquez la configuration à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> avec la méthode <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> :

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrait une valeur à partir de la configuration avec une clé spécifiée et la convertit selon le type spécifié. Une surcharge vous permet de fournir une valeur par défaut si la clé est introuvable.

L’exemple suivant extrait la valeur de la chaîne à partir de la configuration avec la clé `NumberKey`, tape la valeur en tant que `int` et stocke la valeur dans la variable `intValue`. Si `NumberKey` est introuvable dans les clés de configuration, `intValue` reçoit la valeur par défaut `99` :

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a>GetSection, GetChildren et Exists

Pour les exemples qui suivent, utilisez le fichier JSON suivant. Quatre clés se trouvent dans deux sections, dont l’une inclut deux sous-sections :

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

Lorsque le fichier est lu dans la configuration, les clés hiérarchiques uniques suivantes sont créées pour stocker les valeurs de la configuration :

* section0:key0
* section0:key1
* section1:key0
* section1:key1
* section2:subsection0:key0
* section2:subsection0:key1
* section2:subsection1:key0
* section2:subsection1:key1

### <a name="getsection"></a>GetSection

[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrait une sous-section de la configuration avec la clé de sous-section spécifiée. `GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Pour retourner un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> contenant uniquement les paires clé-valeur dans `section1`, appelez `GetSection` et fournissez le nom de section :

```csharp
var configSection = _config.GetSection("section1");
```

`configSection` n’a pas de valeur, seulement une clé et un chemin.

De même, pour obtenir les valeurs des clés dans `section2:subsection0`, appelez `GetSection` et fournissez le chemin d’accès de la section :

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection` ne retourne jamais `null`. Si aucune section correspondante n’est trouvée, une valeur `IConfigurationSection` vide est retournée.

Quand `GetSection` retourne une section correspondante, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> n’est pas rempli. <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> et <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> sont retournés quand la section existe.

### <a name="getchildren"></a>GetChildren

Un appel à [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) sur `section2` obtient un `IEnumerable<IConfigurationSection>` qui inclut :

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a>Existe

Utilisez [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) pour déterminer si une section de configuration existe :

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

Compte tenu des données d’exemple, `sectionExists` est `false`, car il n’y a pas de section `section2:subsection2` dans les données de configuration.

::: moniker-end

## <a name="bind-to-a-class"></a>Lier à une classe

La configuration peut être liée à des classes qui représentent des groupes de paramètres associés à l’aide du *modèle d’options*. Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.

Les valeurs de configuration sont retournées sous forme de chaînes, mais le fait d’appeler <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permet la construction d’objets [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object). `Bind` est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

L’exemple d’application contient un modèle `Starship` (*Models/Starship.cs*) :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

La section `starship` du fichier *starship.json* crée la configuration lorsque l’exemple d’application utilise le Fournisseur de configuration JSON pour charger la configuration :

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

Les paires clé-valeur de configuration suivantes sont créées :

| Touche                   | Value                                             |
| --------------------- | ------------------------------------------------- |
| starship:name         | USS Enterprise                                    |
| starship:registry     | NCC-1701                                          |
| starship:class        | Constitution                                      |
| starship:length       | 304.8                                             |
| starship:commissioned | False                                             |
| trademark             | Paramount Pictures Corp. http://www.paramount.com |

L’exemple d’application appelle `GetSection` avec la clé `starship`. Les paires clé-valeur `starship` sont isolées. La méthode `Bind` est appelée sur la sous-section passant une instance de la classe `Starship`. Après avoir lié les valeurs d’instance, l’instance est affectée à une propriété pour le rendu :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="bind-to-an-object-graph"></a>Établir une liaison à un graphe d’objets

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> est capable de lier l’intégralité d’un graphe d’objets POCO. `Bind` est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

L’exemple contient un modèle `TvShow` dont le graphe d’objets inclut les classes `Metadata` et `Actors` (*Models/TvShow.cs*) :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

L’exemple d’application a un fichier *tvshow.xml* contenant les données de configuration :

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

La configuration est liée à l’intégralité du graphe d’objets `TvShow` avec la méthode `Bind`. L’instance liée est affectée à une propriété pour le rendu :

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) lie et retourne le type spécifié. Il est plus pratique d’utiliser `Get<T>` que `Bind`. Le code suivant montre comment utiliser `Get<T>` avec l’exemple précédent, ce qui permet à l’instance liée d’être directement affectée à la propriété utilisée pour le rendu :

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). `Get<T>` est disponible dans ASP.NET Core 1.1 et versions ultérieures. `GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="bind-an-array-to-a-class"></a>Lier un tableau à une classe

*L’exemple d’application illustre les concepts abordés dans cette section.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> prend en charge la liaison de tableaux à des objets à l’aide d’index de tableau dans les clés de configuration. Tout format de tableau qui expose un segment de clé numérique (`:0:`, `:1:`, &hellip; `:{n}:`) est capable d’effectuer la liaison de tableau avec un tableau de classes POCO. « Bind » est dans le package [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

> [!NOTE]
> La liaison est fournie par convention. Les fournisseurs de configuration personnalisés ne sont pas obligés d’implémenter la liaison de tableau.

**Traitement de tableau en mémoire**

Observez les valeurs et les clés de configuration indiquées dans le tableau suivant.

| Touche             | Value  |
| :-------------: | :----: |
| array:entries:0 | value0 |
| array:entries:1 | valeur1 |
| array:entries:2 | valeur2 |
| array:entries:4 | value4 |
| array:entries:5 | value5 |

Ces clés et valeurs sont chargées dans l’exemple d’application à l’aide du Fournisseur de configuration de mémoire :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

Le tableau ignore une valeur pour l’index &num;3. Le binder de configuration n’est pas capable de lier des valeurs null ou de créer des entrées null dans les objets liés, ce qui deviendra clair dans un moment lorsque le résultat de liaison de ce tableau à un objet est illustré.

Dans l’exemple d’application, une classe POCO est disponible pour contenir les données de configuration liées :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

Les données de configuration sont liées à l’objet :

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

`GetSection` est dans le package [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), qui se trouve dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker range=">= aspnetcore-1.1"

La syntaxe [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) peut également être utilisée, ce qui aboutit à un code plus compact :

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

L’objet lié, une instance de `ArrayExample`, reçoit les données de tableau à partir de la configuration.

| Index `ArrayExample.Entries` | Valeur `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | valeur1                       |
| 2                            | valeur2                       |
| 3                            | value4                       |
| 4                            | value5                       |

L’index &num;3 dans l’objet lié contient les données de configuration pour la clé de configuration `array:4` et sa valeur de `value4`. Lorsque des données de configuration contenant un tableau sont liées, les index de tableau dans les clés de configuration sont simplement utilisés pour itérer les données de configuration lors de la création de l’objet. Une valeur null ne peut pas être conservée dans des données de configuration, et une entrée à valeur null n’est pas créée dans un objet lié quand un tableau dans des clés de configuration ignore un ou plusieurs index.

L’élément de configuration manquant pour l’index &num;3 peut être fourni avant la liaison à l’instance `ArrayExample` par n’importe quel fournisseur de configuration qui génère la paire clé-valeur correcte dans la configuration. Si l’exemple inclut un Fournisseur de configuration JSON supplémentaire avec la paire clé-valeur manquante, `ArrayExample.Entries` correspond à l’intégralité du tableau de configuration :

*missing_value.json* :

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

Dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dans le constructeur `Startup` :

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

La paire clé-valeur indiquée dans le tableau est chargée dans la configuration.

| Touche             | Value  |
| :-------------: | :----: |
| array:entries:3 | valeur3 |

Si l’instance de classe `ArrayExample` est liée une fois que le Fournisseur de configuration JSON inclut l’entrée pour l’index &num;3, le tableau `ArrayExample.Entries` inclut la valeur.

| Index `ArrayExample.Entries` | Valeur `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | valeur1                       |
| 2                            | valeur2                       |
| 3                            | valeur3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**Traitement de tableau JSON**

Si un fichier JSON contient un tableau, les clés de configuration sont créés pour les éléments du tableau avec un index de section basé sur zéro. Dans le fichier de configuration suivant, `subsection` est un tableau :

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

Le Fournisseur de configuration JSON lit les données de configuration dans les paires clé-valeur suivantes :

| Touche                     | Value  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

Dans l’exemple d’application, la classe POCO suivante est disponible pour lier les paires clé-valeur de configuration :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

Après la liaison, `JsonArrayExample.Key` contient la valeur `valueA`. Les valeurs de la sous-section sont stockées dans la propriété de tableau POCO, `Subsection`.

| Index `JsonArrayExample.Subsection` | Valeur `JsonArrayExample.Subsection` |
| :---------------------------------: | :---------------------------------: |
| 0                                   | valueB                              |
| 1                                   | valueC                              |
| 2                                   | valueD                              |

## <a name="custom-configuration-provider"></a>Fournisseur de configuration personnalisé

L’exemple d’application montre comment créer un fournisseur de configuration de base qui lit les paires clé-valeur de configuration à partir d’une base de données à l’aide [d’Entity Framework (EF)](/ef/core/).

Le fournisseur présente les caractéristiques suivantes :

* La base de données en mémoire EF est utilisée à des fins de démonstration. Pour utiliser une base de données qui nécessite une chaîne de connexion, implémentez un autre `ConfigurationBuilder` pour fournir la chaîne de connexion à partir d’un autre fournisseur de configuration.
* Le fournisseur lit une table de base de données dans la configuration au démarrage. Le fournisseur n’interroge pas la base de données par clé.
* Le rechargement en cas de changement n’est pas implémenté, par conséquent, la mise à jour la base de données après le démarrage de l’application n’a aucun effet sur la configuration de l’application.

Définissez une entité `EFConfigurationValue` pour le stockage des valeurs de configuration dans la base de données.

*Models/EFConfigurationValue.cs* :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

Ajoutez un `EFConfigurationContext` pour stocker les valeurs configurées et y accéder.

*EFConfigurationProvider/EFConfigurationContext.cs* :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

Créez une classe qui implémente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource.cs* :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

Créez le fournisseur de configuration personnalisé en héritant de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Le fournisseur de configuration initialise la base de données quand elle est vide.

*EFConfigurationProvider/EFConfigurationProvider.cs* :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

Une méthode d’extension `AddEFConfiguration` permet d’ajouter la source de configuration à un `ConfigurationBuilder`.

*Extensions/EntityFrameworkExtensions.cs* :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

Le code suivant montre comment utiliser le `EFConfigurationProvider` personnalisé dans *Program.cs* :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a>Accéder à la configuration lors du démarrage

Injectez `IConfiguration` dans le constructeur `Startup` pour accéder aux valeurs de configuration dans `Startup.ConfigureServices`. Pour accéder à la configuration dans `Startup.Configure`, injectez `IConfiguration` directement dans la méthode ou utilisez l’instance à partir du constructeur :

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

Pour obtenir un exemple d’accès à la configuration à l’aide des méthodes pratiques de démarrage, consultez [Démarrage de l’application : méthodes pratiques](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Accéder à la configuration dans une page Razor Pages ou une vue MVC

Pour accéder aux paramètres de configuration dans une page Pages Razor ou une vue MVC, ajoutez une [directive using](xref:mvc/views/razor#using) ([Informations de référence sur C# : directive using](/dotnet/csharp/language-reference/keywords/using-directive)) pour [l’espace de noms Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) et injectez <xref:Microsoft.Extensions.Configuration.IConfiguration> dans la page ou la vue.

Dans une page Pages Razor :

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

Dans une vue MVC :

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a>Ajouter la configuration à partir d’un assembly externe

Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application. Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/configuration/options>
* [Présentation approfondie de la Configuration Microsoft](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
