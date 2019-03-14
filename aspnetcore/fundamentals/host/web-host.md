---
title: Hôte web ASP.NET Core
author: guardrex
description: Découvrez l’hôte web dans ASP.NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 878fbaa1a61946dadf23ba8fefbf22021e547cc2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044946"
---
# <a name="aspnet-core-web-host"></a>Hôte web ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Les applications ASP.NET Core configurent et lancent un *hôte*. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications. Au minimum, l’hôte configure un serveur ainsi qu’un pipeline de traitement des requêtes. L’hôte peut aussi configurer la journalisation, l’injection de dépendances et la configuration.

::: moniker range="<= aspnetcore-1.1"

Pour obtenir la version 1.1 de cette rubrique, téléchargez [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Cet article traite de l’hôte web ASP.NET Core (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), qui est responsable de l’hébergement des applications web. Pour obtenir des informations sur l’hôte générique .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), consultez <xref:fundamentals/host/generic-host>.

::: moniker-end

::: moniker range="> aspnetcore-2.2"

Cet article traite de l’hôte web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)). Dans ASP.NET Core 3.0, l’hôte générique remplace l’hôte web. Pour plus d’informations, consultez [L’hôte](xref:fundamentals/index#host).

::: moniker-end

## <a name="set-up-a-host"></a>Configurer un hôte

Créez un hôte en utilisant une instance de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`. Dans les modèles de projet, `Main` se trouve dans *Program.cs*. Un fichier *Program.cs* standard appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour commencer la configuration d’un hôte :

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

`CreateDefaultBuilder` effectue les tâches suivantes :

* Configure le serveur [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web en utilisant les fournisseurs de configuration d’hébergement de l’application. Pour connaître les options par défaut du serveur Kestrel, consultez <xref:fundamentals/servers/kestrel#kestrel-options>.
* Définit le chemin retourné par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) comme racine de contenu.
* Charge la [configuration de l’hôte](#host-configuration-values) à partir de :
  * Variables d’environnement comportant le préfixe `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`).
  * Arguments de ligne de commande
* Charge la configuration de l’application dans l’ordre suivant à partir des éléments ci-après :
  * *appsettings.json*
  * *appsettings.{Environment}.json*
  * L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.
  * Variables d'environnement.
  * Arguments de ligne de commande
* Configure la [journalisation](xref:fundamentals/logging/index) des sorties de la console et du débogage. La journalisation inclut les règles de [filtrage de journal](xref:fundamentals/logging/index#log-filtering) qui sont spécifiées dans une section de configuration de la journalisation dans un fichier *appsettings.json* ou *appsettings.{Environment}.json*.
* Lors de l’exécution derrière IIS avec le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` permet l’[intégration IIS](xref:host-and-deploy/iis/index), qui configure l’adresse de base et le port de l’application. L’intégration IIS configure également l’application pour la [capture des erreurs de démarrage](#capture-startup-errors). Pour connaître les options par défaut d’IIS, consultez <xref:host-and-deploy/iis/index#iis-options>.
* Définissez [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) sur `true` si l’environnement de l’application est Développement. Pour plus d’informations, consultez [Validation de l’étendue](#scope-validation).

La configuration définie par `CreateDefaultBuilder` peut être remplacée et enrichie par [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) et les autres méthodes et les méthodes d’extension de [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). En voici quelques exemples :

* [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) permet de spécifier une configuration `IConfiguration` supplémentaire pour l’application. L’appel `ConfigureAppConfiguration` suivant ajoute un délégué pour inclure la configuration de l’application dans le fichier *appsettings.xml*. `ConfigureAppConfiguration` peut être appelé plusieurs fois. Notez que cette configuration ne s’applique pas à l’hôte (par exemple, les URL de serveur ou l’environnement). Voir la section [Valeurs de configuration de l’hôte](#host-configuration-values).

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* L’appel `ConfigureLogging` suivant ajoute un délégué pour configurer le niveau de journalisation minimal ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) sur [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel). Ce paramètre remplace les paramètres de *appsettings.Development.json* (`LogLevel.Debug`) et *appsettings.Production.json* (`LogLevel.Error`) configurés par `CreateDefaultBuilder`. `ConfigureLogging` peut être appelé plusieurs fois.

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* L’appel suivant à `ConfigureKestrel` remplace la valeur par défaut de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) (30 000 000 octets) établie lors de la configuration de Kestrel par `CreateDefaultBuilder` :

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* L’appel suivant à [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) remplace la valeur par défaut de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) (30 000 000 octets) établie lors de la configuration de Kestrel par `CreateDefaultBuilder` :

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC. Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu. Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).

Pour plus d’informations sur la configuration d’application, consultez <xref:fundamentals/configuration/index>.

> [!NOTE]
> Au lieu d’utiliser la méthode statique `CreateDefaultBuilder`, vous pouvez aussi créer un hôte à partir de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette approche est prise en charge dans ASP.NET Core 2.x. Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.

Lors de la configuration d’un hôte, les méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) peuvent être fournies. Si une classe `Startup` est spécifiée, elle doit définir une méthode `Configure`. Pour plus d'informations, consultez <xref:fundamentals/startup>. Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres. Les appels multiples à `Configure` ou `UseStartup` sur `WebHostBuilder` remplacent les paramètres précédents.

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :

* Configuration du générateur de l’hôte, qui inclut des variables d’environnement au format `ASPNETCORE_{configurationKey}`. Par exemple, `ASPNETCORE_ENVIRONMENT`.
* Des extensions comme [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) et [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (voir la section [Remplacer la configuration](#override-configuration)).
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) et la clé associée. Quand une valeur est définie avec `UseSetting`, elle est définie au format chaîne indépendamment du type.

L’hôte utilise l’option qui définit une valeur en dernier. Pour plus d’informations, consultez [Remplacer une configuration](#override-configuration) dans la section suivante.

### <a name="application-key-name"></a>Clé d’application (Nom)

La propriété [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) est définie automatiquement quand [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) ou [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) est appelé pendant la construction de l’hôte. La valeur affectée correspond au nom de l’assembly contenant le point d’entrée de l’application. Pour définir la valeur explicitement, utilisez [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey) :

**Clé** : applicationName  
**Type** : *string*  
**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.  
**Définition avec** : `UseSetting`  
**Variable d’environnement** : `ASPNETCORE_APPLICATIONNAME`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a>Capture des erreurs de démarrage

Ce paramètre contrôle la capture des erreurs de démarrage.

**Clé** : captureStartupErrors  
**Type** : *bool* (`true` ou `1`)  
**Par défaut** : `false`, ou `true` si l’application s’exécute avec Kestrel derrière IIS.  
**Définition avec** : `CaptureStartupErrors`  
**Variable d’environnement** : `ASPNETCORE_CAPTURESTARTUPERRORS`

Avec la valeur `false`, la survenue d’erreurs au démarrage entraîne la fermeture de l’hôte. Avec la valeur `true`, l’hôte capture les exceptions levées au démarrage et tente de démarrer le serveur.

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a>Racine de contenu

Ce paramètre détermine l’emplacement où ASP.NET Core commence la recherche des fichiers de contenu, tels que les vues MVC. 

**Clé** : contentRoot  
**Type** : *string*  
**Par défaut** : dossier où réside l’assembly de l’application.  
**Définition avec** : `UseContentRoot`  
**Variable d’environnement** : `ASPNETCORE_CONTENTROOT`

La racine de contenu est également utilisée comme chemin de base pour le [paramètre de la racine Web](#web-root). Si le chemin est introuvable, l’hôte ne peut pas démarrer.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a>Erreurs détaillées

Détermine si les erreurs détaillées doivent être capturées.

**Clé** : detailedErrors  
**Type** : *bool* (`true` ou `1`)  
**Valeur par défaut** : false  
**Définition avec** : `UseSetting`  
**Variable d’environnement** : `ASPNETCORE_DETAILEDERRORS`

Quand cette fonctionnalité est activée (ou que <a href="#environment">l’environnement</a> est défini sur `Development`), l’application capture les exceptions détaillées.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a>Environnement

Définit l’environnement de l’application.

**Clé** : environment  
**Type** : *string*  
**Par défaut** : Production  
**Définition avec** : `UseEnvironment`  
**Variable d’environnement** : `ASPNETCORE_ENVIRONMENT`

L’environnement peut être défini à n’importe quelle valeur. Les valeurs définies par le framework sont `Development`, `Staging` et `Production`. Les valeurs ne respectent pas la casse. Par défaut, *l’environnement* est fourni par la variable d’environnement `ASPNETCORE_ENVIRONMENT`. Si vous utilisez [Visual Studio](https://www.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *launchSettings.json*. Pour plus d'informations, consultez <xref:fundamentals/environments>.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a>Assemblys d’hébergement au démarrage

Définit les assemblys d’hébergement au démarrage de l’application.

**Clé** : hostingStartupAssemblies  
**Type** : *string*  
**Par défaut** : Chaîne vide  
**Définition avec** : `UseSetting`  
**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à charger au démarrage.

La valeur de configuration par défaut est une chaîne vide, mais les assemblys d’hébergement au démarrage incluent toujours l’assembly de l’application. Quand des assemblys d’hébergement au démarrage sont fournis, ils sont ajoutés à l’assembly de l’application et sont chargés lorsque l’application génère ses services communs au démarrage.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a>Port HTTPS

Définissez le port de redirection HTTPS. Utilisé dans [l’application de HTTPS](xref:security/enforcing-ssl).

**Clé** : https_port **Type** : *string*
**Par défaut** : Aucune valeur par défaut n’est définie.
**Définir avec** : `UseSetting`
**La variable d’environnement**: `ASPNETCORE_HTTPS_PORT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a>Assemblys d’hébergement à exclure au démarrage

Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à exclure au démarrage.

**Clé** : hostingStartupExcludeAssemblies  
**Type** : *string*  
**Par défaut** : Chaîne vide  
**Définition avec** : `UseSetting`  
**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a>URL d’hébergement préférées

Indique si l’hôte doit écouter les URL configurées avec `WebHostBuilder` au lieu d’écouter celles configurées avec l’implémentation `IServer`.

**Clé** : preferHostingUrls  
**Type** : *bool* (`true` ou `1`)  
**Valeur par défaut** : true  
**Définition avec** : `PreferHostingUrls`  
**Variable d’environnement** : `ASPNETCORE_PREFERHOSTINGURLS`

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a>Bloquer les assemblys d’hébergement au démarrage

Empêche le chargement automatique des assemblys d’hébergement au démarrage, y compris ceux configurés par l’assembly de l’application. Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.

**Clé** : preventHostingStartup  
**Type** : *bool* (`true` ou `1`)  
**Valeur par défaut** : false  
**Définition avec** : `UseSetting`  
**Variable d’environnement** : `ASPNETCORE_PREVENTHOSTINGSTARTUP`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a>URL du serveur

Indique les adresses IP ou les adresses d’hôte avec les ports et protocoles sur lesquels le serveur doit écouter les requêtes.

**Clé** : urls  
**Type** : *string*  
**Par défaut** : http://localhost:5000  
**Définition avec** : `UseUrls`  
**Variable d’environnement** : `ASPNETCORE_URLS`

Liste de préfixes d’URL séparés par des points-virgules (;) correspondant aux URL auxquelles le serveur doit répondre. Par exemple, `http://localhost:123`. Utilisez « \* » pour indiquer que le serveur doit écouter les requêtes sur toutes les adresses IP ou tous les noms d’hôte qui utilisent le port et le protocole spécifiés (par exemple, `http://*:5000`). Le protocole (`http://` ou `https://`) doit être inclus avec chaque URL. Les formats pris en charge varient selon les serveurs.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

Kestrel a sa propre API de configuration de points de terminaison. Pour plus d'informations, consultez <xref:fundamentals/servers/kestrel#endpoint-configuration>.

### <a name="shutdown-timeout"></a>Délai d’arrêt

Spécifie le délai d’attente avant l’arrêt de l’hôte web.

**Clé** : shutdownTimeoutSeconds  
**Type** : *int*  
**Par défaut** : 5  
**Définition avec** : `UseShutdownTimeout`  
**Variable d’environnement** : `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

La clé accepte une valeur *int* avec `UseSetting` (par exemple, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), mais la méthode d’extension [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) prend une valeur [TimeSpan](/dotnet/api/system.timespan).

Pendant la période du délai d’attente, l’hébergement :

* Déclenche [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).
* Tente d’arrêter les services hébergés, en journalisant les erreurs pour les services qui échouent à s’arrêter.

Si la période du délai d’attente expire avant l’arrêt de tous les services hébergés, les services actifs restants sont arrêtés quand l’application s’arrête. Les services s’arrêtent même s’ils n’ont pas terminé les traitements. Si des services nécessitent plus de temps pour s’arrêter, augmentez le délai d’attente.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a>Assembly de démarrage

Détermine l’assembly à rechercher pour la classe `Startup`.

**Clé** : startupAssembly  
**Type** : *string*  
**Par défaut** : l’assembly de l’application  
**Définition avec** : `UseStartup`  
**Variable d’environnement** : `ASPNETCORE_STARTUPASSEMBLY`

L’assembly peut être référencé par nom (`string`) ou type (`TStartup`). Si plusieurs méthodes `UseStartup` sont appelées, la dernière prévaut.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a>Racine Web

Définit le chemin relatif des ressources statiques de l’application.

**Clé** : webroot  
**Type** : *string*  
**Par défaut** : quand aucune valeur n’est spécifiée, la valeur par défaut est « (Racine de contenu)/wwwroot », si ce chemin existe. Si ce chemin est introuvable, un fournisseur de fichiers no-op est utilisé.  
**Définition avec** : `UseWebRoot`  
**Variable d’environnement** : `ASPNETCORE_WEBROOT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a>Remplacer la configuration

Utilisez [Configuration](xref:fundamentals/configuration/index) pour configurer l’hôte web. Dans l’exemple suivant, la configuration de l’hôte est spécifiée de façon facultative dans un fichier *hostsettings.json*. Toute configuration chargée à partir du fichier *hostsettings.json* est remplaçable par des arguments de ligne de commande. La configuration définie (dans `config`) est utilisée pour configurer l’hôte avec [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration). La configuration `IWebHostBuilder` est ajoutée à la configuration de l’application, mais l’inverse n’est pas vrai &mdash; `ConfigureAppConfiguration` n’a pas d’incidence sur la configuration `IWebHostBuilder`.

La configuration fournie par `UseUrls` est d’abord remplacée par la configuration *hostsettings.json*, puis par la configuration des arguments de ligne de commande :

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
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            });
    }
}
```

*hostsettings.json* :

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> La méthode d’extension [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) ne peut pas analyser une section de configuration retournée par `GetSection` (par exemple, `.UseConfiguration(Configuration.GetSection("section"))`. La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:urls`, `section:environment`). La méthode `UseConfiguration` suppose que les clés correspondent aux clés `WebHostBuilder` (par exemple, `urls`, `environment`). La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte. Ce problème sera résolu dans une prochaine mise en production. Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).
>
> `UseConfiguration` copie seulement les clés de la configuration `IConfiguration` fournie vers la configuration du générateur d’hôte. Par conséquent, le fait de définir `reloadOnChange: true` pour les fichiers de paramètres XML, JSON et INI n’a aucun effet.

Pour spécifier l’exécution de l’hôte sur une URL particulière, vous pouvez passer la valeur souhaitée à partir d’une invite de commandes lors de l’exécution de [dotnet run](/dotnet/core/tools/dotnet-run). L’argument de ligne de commande remplace la valeur `urls` du fichier *hostsettings.json*, et le serveur écoute le port 8080 :

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a>Gérer l’hôte

**Exécuter**

La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :

```csharp
host.Run();
```

**Démarrer**

Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

L’application peut initialiser et démarrer un nouvel hôte ayant les valeurs par défaut préconfigurées de `CreateDefaultBuilder` en utilisant une méthode d’usage statique. Ces méthodes démarrent le serveur sans sortie de console et, avec [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown), elles attendent un arrêt (Ctrl-C/SIGINT ou SIGTERM) :

**Start(RequestDelegate app)**

Commencez par un `RequestDelegate` :

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! » `WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM). L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.

**Start(string url, RequestDelegate app)**

Commencez par une URL et `RequestDelegate` :

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produit le même résultat que **Start(RequestDelegate app)**, sauf que l’application répond sur `http://localhost:8080`.

**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**

Utilisez une instance de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) pour utiliser le middleware de routage :

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Utilisez les requêtes de navigateur suivantes avec l’exemple :

| Demande                                    | Réponse                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Lève une exception avec la chaîne « ooops! » |
| `http://localhost:5000/throw`              | Lève une exception avec la chaîne « Uh oh! » |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM). L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.

**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**

Utilisez une URL et une instance de `IRouteBuilder` :

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Produit le même résultat que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, sauf que l’application répond sur `http://localhost:8080`.

**StartWith(Action&lt;IApplicationBuilder&gt; app)**

Fournissez un délégué pour configurer un `IApplicationBuilder` :

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! » `WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM). L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.

**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**

Fournissez une URL et un délégué pour configurer un `IApplicationBuilder` :

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Produit le même résultat que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, sauf que l’application répond sur `http://localhost:8080`.

## <a name="ihostingenvironment-interface"></a>Interface IHostingEnvironment

[L’interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement web de l’application. Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

Vous pouvez utiliser une [approche basée sur une convention](xref:fundamentals/environments#environment-based-startup-class-and-methods) pour configurer l’application au démarrage en fonction de l’environnement. Vous pouvez également injecter `IHostingEnvironment` dans le constructeur `Startup` pour l’utiliser dans `ConfigureServices` :

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> En plus de la méthode d’extension `IsDevelopment`, `IHostingEnvironment` fournit les méthodes `IsStaging`, `IsProduction` et `IsEnvironment(string environmentName)`. Pour plus d'informations, consultez <xref:fundamentals/environments>.

Le service `IHostingEnvironment` peut également être injecté directement dans la méthode `Configure` pour configurer le pipeline de traitement :

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

`IHostingEnvironment` peut être injecté dans la méthode `Invoke` lors de la création d’un [intergiciel (middleware)](xref:fundamentals/middleware/write) personnalisé :

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>Interface IApplicationLifetime

[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) s’utilise pour les opérations post-démarrage et arrêt. Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.

| Jeton d’annulation    | Événement déclencheur |
| --------------------- | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | L’hôte a complètement démarré. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | L’hôte effectue un arrêt approprié. Toutes les requêtes doivent être traitées. L’arrêt est bloqué tant que cet événement n’est pas terminé. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | L’hôte effectue un arrêt approprié. Des requêtes peuvent encore être en cours de traitement. L’arrêt est bloqué tant que cet événement n’est pas terminé. |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application. La classe suivante utilise `StopApplication` pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a>Validation de l’étendue

[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) affecte la valeur `true` à [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) si l’environnement de l’application est Développement.

Quand `ValidateScopes` est défini sur `true`, le fournisseur de services par défaut vérifie que :

* Les services délimités ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.
* Les services Scoped ne sont pas directement ou indirectement injectés dans des singletons.

Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé. La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.

Les services Scoped sont supprimés par le conteneur qui les a créés. Si un service Scoped est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté. La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.

Pour toujours valider les étendues, notamment dans l’environnement de production, configurez [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) avec [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) sur le créateur d’hôte :

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
