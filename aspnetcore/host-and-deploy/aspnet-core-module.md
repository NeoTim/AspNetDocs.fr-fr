---
title: Module ASP.NET Core
author: guardrex
description: Découvrez comment configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 302cfb00127c223aeb5e51e4d0a9ef3cb69b10eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029886"
---
# <a name="aspnet-core-module"></a>Module ASP.NET Core

Par [Nowak](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik) et [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-2.2"

Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour effectuer l’une des opérations suivantes :

* Héberger une application ASP.NET Core à l’intérieur du processus de travail IIS (`w3wp.exe`), appelé [modèle d’hébergement in-process](#in-process-hosting-model).
* Transférer les requêtes web à une application ASP.NET Core du back-end exécutant le [serveur Kestrel](xref:fundamentals/servers/kestrel), appelé [modèle d’hébergement out-of-process](#out-of-process-hosting-model).

Versions Windows prises en charge :

* Windows 7 ou version ultérieure
* Windows Server 2008 R2 ou une version ultérieure

Lors de l’hébergement in-process, le module utilise une implémentation du serveur in-process pour IIS, nommée serveur HTTP IIS (`IISHttpServer`).

Lors de l’hébergement out-of-process, le module fonctionne uniquement avec Kestrel. Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys).

## <a name="hosting-models"></a>Modèles d'hébergement

### <a name="in-process-hosting-model"></a>Modèle d’hébergement in-process

Pour configurer l’hébergement in-process dans une application, ajoutez la propriété `<AspNetCoreHostingModel>` au fichier projet de l’application en lui attribuant la valeur `InProcess` (l’hébergement out-of-process est défini sur `OutOfProcess`) :

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Le modèle d’hébergement in-process n’est pas pris en charge pour les applications ASP.NET Core qui ciblent le .NET Framework.

Si la propriété `<AspNetCoreHostingModel>` n’est pas présente dans le fichier, la valeur par défaut est `OutOfProcess`.

Les caractéristiques suivantes s’appliquent lors de l’hébergement in-process :

* Le serveur HTTP IIS (`IISHttpServer`) est utilisé à la place du serveur [Kestrel](xref:fundamentals/servers/kestrel). Pour in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> pour :

  * Enregistrer `IISHttpServer`.
  * Configurer le port et le chemin de base sur lesquels le serveur doit écouter lors de l’exécution derrière le module ASP.NET Core.
  * Configurer l’hôte pour capturer des erreurs de démarrage.

* L’[attribut requestTimeout](#attributes-of-the-aspnetcore-element) ne s’applique pas à l’hébergement in-process.

* Le partage d’un pool d’applications entre les applications n’est pas pris en charge. Utilisez un pool d’applications par application.

* Lorsque vous utilisez [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ou que vous placez manuellement un [fichier app_offline.htm dans le déploiement](xref:host-and-deploy/iis/index#locked-deployment-files), l’application peut ne pas s’arrêter immédiatement si une connexion est ouverte. Par exemple, une connexion websocket peut retarder l’arrêt de l’application.

* L’architecture (nombre de bits) de l’application et le runtime installé (x64 ou x86) doivent correspondre à l’architecture du pool d’applications.

* Si vous configurez manuellement l’hôte de l’application avec `WebHostBuilder` (et non [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)), et que l’application est exécutée directement sur le serveur Kestrel (auto-hébergée), appelez `UseKestrel` avant d’appeler `UseIISIntegration`. Si l’ordre est inversé, l’hôte ne parvient pas à démarrer.

* Les déconnexions du client sont détectées. Le jeton d’annulation [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) est annulé quand le client se déconnecte.

* Dans ASP.NET Core version 2.2.1 ou antérieure, <xref:System.IO.Directory.GetCurrentDirectory*> retourne le répertoire de travail du processus démarré par IIS, et non le répertoire de l’application (par exemple, *C:\Windows\System32\inetsrv* pour *w3wp.exe*).

  Pour connaître l’exemple de code qui définit le répertoire actuel de l’application, consultez [CurrentDirectoryHelpers, classe](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs). Appelez la méthode `SetCurrentDirectory`. Les appels suivants à <xref:System.IO.Directory.GetCurrentDirectory*> fournissent le répertoire de l’application.
  
* Dans le cas d’un hébergement in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelé en interne pour initialiser un utilisateur. Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut. Si vous transformez les revendications avec une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, appelez <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> pour ajouter des services d’authentification :

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }
  
  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a>Modèle d’hébergement out-of-process

Pour configurer une application pour un hébergement out-of-process, utilisez l’une des approches suivantes dans le fichier de projet :

* Ne spécifiez pas la propriété `<AspNetCoreHostingModel>`. Si la propriété `<AspNetCoreHostingModel>` n’est pas présente dans le fichier, la valeur par défaut est `OutOfProcess`.
* Définissez la valeur de la propriété `<AspNetCoreHostingModel>` sur `OutOfProcess` (l’hébergement in-process est défini avec `InProcess`) :

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Le serveur [Kestrel](xref:fundamentals/servers/kestrel) est utilisé à la place du serveur HTTP IIS (`IISHttpServer`).

Pour out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pour :

* Configurer le port et le chemin de base sur lesquels le serveur doit écouter lors de l’exécution derrière le module ASP.NET Core.
* Configurer l’hôte pour capturer des erreurs de démarrage.

### <a name="hosting-model-changes"></a>Modifications du modèle d’hébergement

Si le paramètre `hostingModel` est modifié dans le fichier *web.config* (comme expliqué dans la section [Configuration avec web.config](#configuration-with-webconfig)), le module recycle le processus de travail pour IIS.

Pour IIS Express, le module ne recycle pas le processus de travail, mais déclenche plutôt un arrêt approprié du processus IIS Express en cours. La requête suivante à l’application génère un nouveau processus IIS Express.

### <a name="process-name"></a>Nom du processus

`Process.GetCurrentProcess().ProcessName` signale `w3wp`/`iisexpress` (in-process) ou `dotnet` (out-of-process).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour transférer les requêtes web aux applications ASP.NET Core du back-end.

Versions Windows prises en charge :

* Windows 7 ou version ultérieure
* Windows Server 2008 R2 ou une version ultérieure

Le module fonctionne seulement avec Kestrel. Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys).

Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module traite également la gestion des processus. Le module démarre le processus de l’application ASP.NET Core quand la première requête arrive et redémarre l’application si elle plante. Il s’agit essentiellement du même comportement que celui des applications ASP.NET 4.x qui s’exécutent in-process dans IIS et sont gérées par le [service WAS (Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application :

![Module ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Les requêtes arrivent du web au pilote HTTP.sys en mode noyau. Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS). Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est ni le port 80 ni le port 443.

Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`. Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées. Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.

Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core. Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application. Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel. La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.

::: moniker-end

De nombreux modules natifs, comme l’authentification Windows, restent actifs. Pour obtenir plus d’informations sur les modules IIS actifs avec le module ASP.NET Core, consultez <xref:host-and-deploy/iis/modules>.

Le module ASP.NET Core peut également  :

* Définir des variables d’environnement pour le processus de travail.
* Journaliser la sortie de stdout dans un stockage de fichiers pour résoudre les problèmes de démarrage.
* Transférer des jetons d’authentification Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Comment installer et utiliser le module ASP.NET Core

Pour obtenir des instructions sur l’installation et l’utilisation du module ASP.NET Core, consultez <xref:host-and-deploy/iis/index>.

## <a name="configuration-with-webconfig"></a>Configuration avec web.config

Le module ASP.NET Core est configuré avec la section `aspNetCore` du nœud `system.webServer` dans le fichier *web.config* du site.

Le fichier *web.config* suivant est publié pour un [déploiement dépendant du framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) et configure le module ASP.NET Core pour gérer les demandes du site :

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Le fichier *web.config* suivant est publié pour un [déploiement autonome](/dotnet/articles/core/deploying/#self-contained-deployments-scd) :

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

La propriété <xref:System.Configuration.SectionInformation.InheritInChildApplications*> est définie sur `false` pour indiquer que les paramètres spécifiés dans l’élément [\<emplacement>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) ne sont pas hérités par les applications qui se trouvent dans un sous-répertoire de l’application.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Lorsqu’une application est déployée sur [Azure App Service](https://azure.microsoft.com/services/app-service/), le chemin d’accès `stdoutLogFile` est défini sur `\\?\%home%\LogFiles\stdout`. Le chemin d’accès enregistre les journaux stdout dans le dossier *LogFiles*, un emplacement automatiquement créé par le service.

Pour plus d’informations sur la configuration d’une sous-application IIS, consultez <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Attributs de l’élément aspNetCore

::: moniker range=">= aspnetcore-2.2"

| Attribut | Description | Par défaut |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attribut de chaîne facultatif.</p><p>Arguments pour l’exécutable spécifié dans **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande. Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</p> | `true` |
| `hostingModel` | <p>Attribut de chaîne facultatif.</p><p>Spécifie le modèle d’hébergement comme étant in-process (`InProcess`) ou out-of-process (`OutOfProcess`).</p> | `OutOfProcess` |
| `processesPerApplication` | <p>Attribut entier facultatif.</p><p>Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</p><p>&dagger;Pour l’hébergement in-process, la valeur est limitée à `1`.</p><p>Il est déconseillé de définir `processesPerApplication`. Cet attribut sera supprimé dans une version ultérieure.</p> | Par défaut : `1`<br>Min : `1`<br>Max : `100`&dagger; |
| `processPath` | <p>Attribut de chaîne requis.</p><p>Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP. Les chemins d’accès relatifs sont pris en charge. Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</p> | |
| `rapidFailsPerMinute` | <p>Attribut entier facultatif.</p><p>Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute. Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</p><p>Non pris en charge avec hébergement in-process.</p> | Par défaut : `10`<br>Min : `0`<br>Max : `100` |
| `requestTimeout` | <p>Attribut timespan facultatif.</p><p>Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</p><p>Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</p><p>Ne s’applique pas à l’hébergement in-process. Pour l’hébergement in-process, le module attend que l’application traite la requête.</p> | Par défaut : `00:02:00`<br>Min : `00:00:00`<br>Max : `360:00:00` |
| `shutdownTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</p> | Par défaut : `10`<br>Min : `0`<br>Max : `600` |
| `startupTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port. Si cette limite de temps est dépassée, le module met fin au processus. Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</p><p>La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</p> | Par défaut : `120`<br>Min : `0`<br>Max : `3600` |
| `stdoutLogEnabled` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attribut de chaîne facultatif.</p><p>Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés. Les chemins d’accès relatifs sont relatifs par rapport à la racine du site. Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus. Tous les dossiers fournis dans le chemin sont créés par le module au moment de la création du fichier journal. Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**. Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Attribut | Description | Par défaut |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attribut de chaîne facultatif.</p><p>Arguments pour l’exécutable spécifié dans **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande. Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</p> | `true` |
| `processesPerApplication` | <p>Attribut entier facultatif.</p><p>Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</p><p>Il est déconseillé de définir `processesPerApplication`. Cet attribut sera supprimé dans une version ultérieure.</p> | Par défaut : `1`<br>Min : `1`<br>Max : `100` |
| `processPath` | <p>Attribut de chaîne requis.</p><p>Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP. Les chemins d’accès relatifs sont pris en charge. Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</p> | |
| `rapidFailsPerMinute` | <p>Attribut entier facultatif.</p><p>Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute. Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</p> | Par défaut : `10`<br>Min : `0`<br>Max : `100` |
| `requestTimeout` | <p>Attribut timespan facultatif.</p><p>Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</p><p>Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</p> | Par défaut : `00:02:00`<br>Min : `00:00:00`<br>Max : `360:00:00` |
| `shutdownTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</p> | Par défaut : `10`<br>Min : `0`<br>Max : `600` |
| `startupTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port. Si cette limite de temps est dépassée, le module met fin au processus. Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</p><p>La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</p> | Par défaut : `120`<br>Min : `0`<br>Max : `3600` |
| `stdoutLogEnabled` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attribut de chaîne facultatif.</p><p>Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés. Les chemins d’accès relatifs sont relatifs par rapport à la racine du site. Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus. Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal. Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**. Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| Attribut | Description | Par défaut |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attribut de chaîne facultatif.</p><p>Arguments pour l’exécutable spécifié dans **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande. Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</p> | `true` |
| `processesPerApplication` | <p>Attribut entier facultatif.</p><p>Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</p><p>Il est déconseillé de définir `processesPerApplication`. Cet attribut sera supprimé dans une version ultérieure.</p> | Par défaut : `1`<br>Min : `1`<br>Max : `100` |
| `processPath` | <p>Attribut de chaîne requis.</p><p>Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP. Les chemins d’accès relatifs sont pris en charge. Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</p> | |
| `rapidFailsPerMinute` | <p>Attribut entier facultatif.</p><p>Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute. Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</p> | Par défaut : `10`<br>Min : `0`<br>Max : `100` |
| `requestTimeout` | <p>Attribut timespan facultatif.</p><p>Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</p><p>Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.0 ou version antérieure, la durée `requestTimeout` doit être spécifiée en minutes entières, sinon la valeur par défaut est de 2 minutes.</p> | Par défaut : `00:02:00`<br>Min : `00:00:00`<br>Max : `360:00:00` |
| `shutdownTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</p> | Par défaut : `10`<br>Min : `0`<br>Max : `600` |
| `startupTimeLimit` | <p>Attribut entier facultatif.</p><p>Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port. Si cette limite de temps est dépassée, le module met fin au processus. Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</p> | Par défaut : `120`<br>Min : `0`<br>Max : `3600` |
| `stdoutLogEnabled` | <p>Attribut booléen facultatif.</p><p>Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attribut de chaîne facultatif.</p><p>Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés. Les chemins d’accès relatifs sont relatifs par rapport à la racine du site. Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus. Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal. Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**. Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Définition des variables d'environnement

::: moniker range=">= aspnetcore-3.0"

Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`. Spécifiez une variable d’environnement à l’aide de l’élément enfant `<environmentVariable>` d’un élément de collection `<environmentVariables>`. Les variables d’environnement définies dans cette section prévalent sur les variables d’environnement système.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`. Spécifiez une variable d’environnement à l’aide de l’élément enfant `<environmentVariable>` d’un élément de collection `<environmentVariables>`.

> [!WARNING]
> Les variables d’environnement définies dans cette section sont en conflit avec les variables d’environnement système qui ont le même nom. Si une variable d’environnement est définie à la fois dans le fichier *web.config* et au niveau du système dans Windows, la valeur provenant du fichier *web.config* est ajoutée à la valeur de la variable d’environnement système (par exemple, `ASPNETCORE_ENVIRONMENT: Development;Development`), ce qui empêche le démarrage de l’application.

::: moniker-end

L’exemple suivant définit deux variables d'environnement. `ASPNETCORE_ENVIRONMENT` définit l’environnement de l’application sur `Development`. Un développeur peut définir temporairement cette valeur dans le fichier *web.config* afin de forcer le chargement de la [Page d’exception de développeur](xref:fundamentals/error-handling) lors du débogage d’une exception d’application. `CONFIG_DIR` est un exemple de variable d’environnement définie par l’utilisateur, dans laquelle le développeur a écrit du code qui lit la valeur de démarrage afin de former un chemin d’accès pour le chargement du fichier de configuration de l’application.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Une alternative à la définition directe de l’environnement dans le fichier *web.config* consiste à inclure la propriété `<EnvironmentName>` dans le profil de publication (*.pubxml*) ou le fichier projet. Cette approche définit l’environnement dans *web.config* lorsque le projet est publié :
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> Affectez uniquement `Development` à la variable d’environnement `ASPNETCORE_ENVIRONMENT` sur les serveurs de test et de préproduction non accessibles aux réseaux non approuvés, par exemple Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Si un fichier portant le nom *app_offline.htm* est détecté dans le répertoire racine d’une application, le module ASP.NET Core tente d’arrêter normalement l’application, puis interrompt le traitement des requêtes entrantes. Si l’application est toujours active après le nombre de secondes défini dans `shutdownTimeLimit`, le module ASP.NET Core met fin au processus en cours d’exécution.

Tant que le fichier *app_offline.htm* est présent, le module ASP.NET Core répond aux requêtes en renvoyant le contenu du fichier *app_offline.htm*. Lorsque le fichier *app_offline.htm* est supprimé, la requête suivante démarre l’application.

::: moniker range=">= aspnetcore-2.2"

Lorsque vous utilisez le modèle d’hébergement out-of-process, l’application peut ne pas s’arrêter immédiatement s’il existe une connexion ouverte. Par exemple, une connexion websocket peut retarder l’arrêt de l’application.

::: moniker-end

## <a name="start-up-error-page"></a>Page d’erreur de démarrage

::: moniker range=">= aspnetcore-2.2"

Les hébergements in-process et out-of-process produisent des pages d’erreurs personnalisées quand ils ne parviennent pas à démarrer l’application.

Si le module ASP.NET Core ne trouve pas le gestionnaire de requêtes in-process ou out-of-process, une page de code d’état *500.0 - Échec de chargement du gestionnaire in-process/out-of-process* s’affiche.

Pour l’hébergement in-process, si le module ASP.NET Core ne peut pas démarrer l’application, une page de code d’état *500.30 - Échec de démarrage* s’affiche.

Pour l’hébergement out-of-process, si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.

Pour supprimer cette page et revenir à la page IIS de codes d’état 5xx par défaut, utilisez l’attribut `disableStartUpErrorPage`. Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche. Pour supprimer cette page et revenir à la page IIS de code d’état 502 par défaut, utilisez l’attribut `disableStartUpErrorPage`. Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).

![Page de code d’état 502.5 Échec du processus](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a>Création et redirection de journal

Le module ASP.NET Core redirige la sortie de console stdout et stderr vers le disque si les attributs `stdoutLogEnabled` et `stdoutLogFile` de l’élément `aspNetCore` sont définis. Tous les dossiers du chemin `stdoutLogFile` sont créés par le module au moment de la création du fichier journal. Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits (utilisez `IIS AppPool\<app_pool_name>` pour fournir les autorisations d’écriture).

Aucune rotation n’est appliquée aux journaux, sauf en cas de recyclage/redémarrage du processus. Il incombe à l’hébergeur de limiter l’espace disque utilisé par les journaux.

L’utilisation du journal stdout est uniquement recommandée pour résoudre les problèmes de démarrage de l’application. N’utilisez pas le journal stdout à des fins de journalisation d’application générale. Pour journaliser la routine d’une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et appliquez une rotation aux journaux. Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).

Un horodatage et une extension de fichier sont ajoutés automatiquement quand le fichier journal est créé. Le nom du fichier journal est créé en ajoutant l’horodatage, un ID de processus et une extension de fichier (*.log*) au dernier segment du chemin d'accès `stdoutLogFile` (généralement *stdout*), séparés par des traits de soulignement. Si le chemin d'accès `stdoutLogFile` se termine par *stdout*, un journal pour une application avec un PID de 1934 créé le 5/2/2018 à 19:42:32 affiche le nom de fichier *stdout_20180205194132_1934.log*.

::: moniker range=">= aspnetcore-2.2"

Si `stdoutLogEnabled` a la valeur false, les erreurs qui se produisent au moment du démarrage de l’application sont capturées et émises dans le journal des événements (30 Ko maximum). Après le démarrage, tous les journaux supplémentaires sont ignorés.

::: moniker-end

L’exemple d’élément `aspNetCore` suivant configure la journalisation stdout pour une application hébergée dans Azure App Service. Un chemin d’accès local ou un chemin de partage réseau peut être utilisé pour une journalisation locale. Vérifiez que l’identité de l’utilisateur AppPool à l’autorisation d’écrire dans le chemin d’accès fourni.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a>Journaux de diagnostic améliorés

Le module ASP.NET Core est configurable pour proposer des journaux de diagnostic améliorés. Ajoutez l’élément `<handlerSettings>` à l’élément `<aspNetCore>` dans *web.config*. L’affectation de la valeur `TRACE` à `debugLevel` expose une fidélité plus élevée des informations de diagnostic :

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

Les valeurs du niveau de débogage (`debugLevel`) peuvent inclure à la fois le niveau et l’emplacement.

Niveaux (dans l’ordre du moins au plus détaillé) :

* ERROR
* WARNING
* INFO
* TRACE

Emplacements (plusieurs emplacements sont autorisés) :

* CONSOLE
* EVENTLOG
* FICHIER

Les paramètres de gestionnaire peuvent également être fournis par le biais de variables d’environnement :

* `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Chemin du fichier journal de débogage. (Par défaut : *aspnetcore-debug.log*)
* `ASPNETCORE_MODULE_DEBUG` &ndash; Paramètre du niveau de débogage.

> [!WARNING]
> Ne laissez **pas** la journalisation du débogage activée dans le déploiement plus longtemps que nécessaire pour résoudre un problème. La taille du journal n’est pas limitée. Si vous laissez la journalisation du débogage activée, vous risquez d’épuiser l’espace disque disponible et de bloquer le serveur ou le service d’application.

::: moniker-end

Consultez [Configuration avec web.config](#configuration-with-webconfig) pour obtenir un exemple d’élément `aspNetCore` dans le fichier *web.config*.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configuration du proxy utilise le protocole HTTP et un jeton d’appariement

::: moniker range=">= aspnetcore-2.2"

*S’applique uniquement à l’hébergement out-of-process.*

::: moniker-end

Le proxy créé entre le module ASP.NET Core et Kestrel utilise le protocole HTTP. Utiliser HTTP est une optimisation des performances où le trafic entre le module et Kestrel a lieu sur une adresse de bouclage hors de l’interface réseau. Il n’existe aucun risque d’écoute clandestine du trafic entre le module et Kestrel à partir d’un emplacement sur le serveur.

Un jeton d’appariement est utilisé pour garantir que les demandes reçues par Kestrel ont été traitées par IIS et ne proviennent pas d’une autre source. Le jeton d’appariement est créé et défini dans une variable d’environnement (`ASPNETCORE_TOKEN`) par le module. Le jeton d’appariement est également défini dans un en-tête (`MS-ASPNETCORE-TOKEN`) sur toutes les demandes traitées. L'intergiciel IIS vérifie chaque demande qu’il reçoit pour confirmer que la valeur d’en-tête du jeton d'appariement correspond à la valeur de la variable d’environnement. Si les valeurs de jeton ne correspondent pas, la demande est connectée et rejetée. La variable d’environnement du jeton d'appariement et le trafic entre le module et Kestrel ne sont pas accessibles à partir d’un emplacement sur le serveur. Sans connaître la valeur du jeton d'appariement, une personne malveillante ne peut pas soumettre des demandes qui contournent la vérification de l’intergiciel IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Module ASP.NET Core avec une configuration partagée IIS

Le programme d’installation du module ASP.NET Core s’exécute avec les privilèges du compte **TrustedInstaller**. Comme le compte système local n’a pas l’autorisation de modifier le chemin de partage utilisé par la configuration partagée IIS, le programme d’installation génère une erreur d’accès refusé pendant la tentative de configuration des paramètres du module dans le fichier *applicationHost.config* du partage.

::: moniker range=">= aspnetcore-2.2"

Quand une configuration partagée IIS est utilisée sur la même machine que l’installation IIS, il convient d’exécuter le programme d’installation du bundle d’hébergement ASP.NET Core avec le paramètre `OPT_NO_SHARED_CONFIG_CHECK` défini sur `1` :

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Quand le chemin de la configuration partagée ne se trouve pas sur la même machine que l’installation IIS, effectuez ces étapes :

1. Désactivez la configuration partagée IIS.
1. Exécutez le programme d’installation.
1. Exportez le fichier *applicationHost.config* mis à jour vers le partage.
1. Réactivez la configuration partagée IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Lorsque vous utilisez une configuration partagée IIS, procédez comme suit :

1. Désactivez la configuration partagée IIS.
1. Exécutez le programme d’installation.
1. Exportez le fichier *applicationHost.config* mis à jour vers le partage.
1. Réactivez la configuration partagée IIS.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a>Initialisation d’application

[L’Initialisation d’application IIS](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) est une fonctionnalité IIS qui envoie une requête HTTP à l’application lorsque le pool d’applications démarre ou est recyclé. La requête déclenche le démarrage de l’application. L’Initialisation de l’application peut être utilisée à la fois par le [modèle d’hébergement in-process](xref:fundamentals/servers/index#in-process-hosting-model) et ke [modèle d’hébergement out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model) avec le module ASP.NET Core version 2.

Pour activer l’Initialisation d’application :

1. Vérifiez que la fonctionnalité de rôle Initialisation d’application IIS est activée :
   * Sur Windows 7 ou version ultérieure : Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran). Ouvrez **Internet Information Services** > **Services World Wide Web** > **Fonctionnalités de développement d’applications**. Cochez la case **Initialisation d’application**.
   * Sur Windows Server 2008 R2 ou version ultérieure, ouvrez **l’assistant Ajouter des rôles et des fonctionnalités**. Lorsque vous atteignez le panneau **Sélectionner des services de rôle**, ouvrez le nœud **Développement d’applications** et cochez la case **Initialisation d’application**.
1. Dans IIS Manager, sélectionnez **Pools d’applications** dans le volet **Connexions**.
1. Sélectionnez le pool d’applications de l’application dans la liste.
1. Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.
1. Définissez **Mode de démarrage** sur **AlwaysRunning**.
1. Ouvrez le nœud **Sites** dans le panneau **Connexions**.
1. Sélectionnez l’application.
1. Sélectionnez **Paramètres avancés** sous **Gérer le site web** dans le volet **Actions**.
1. Définissez **Préchargement activé** sur **True**.

Pour plus d’informations, consultez [Initialisation d’application IIS 8.0](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).

Les applications qui utilisent le [modèle d’hébergement out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model) doivent utiliser un service externe pour effectuer régulièrement un test ping de l’application afin de garantir son fonctionnement.

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Version du module et journaux du programme d’installation du bundle d’hébergement

Pour déterminer la version du module ASP.NET Core installé :

1. Sur le système hôte, accédez à *%windir%\System32\inetsrv*.
1. Recherchez le fichier *aspnetcore.dll*.
1. Cliquez avec le bouton droit sur le fichier, puis sélectionnez **Propriétés** dans le menu contextuel.
1. Sélectionnez l'onglet **Détails**. Le **version du fichier** et la **version du produit** représentent la version installée du module.

Les journaux du programme d’installation du bundle d’hébergement du module se trouvent dans *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Le fichier est nommé *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Emplacements des fichiers du module, du schéma et de configuration

### <a name="module"></a>Module

**IIS (x86/amd64) :**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

::: moniker range=">= aspnetcore-2.2"

   * %ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

   * %ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

::: moniker-end

**IIS Express (x86/amd64) :**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

::: moniker range=">= aspnetcore-2.2"

   * %ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

   * %ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

::: moniker-end

### <a name="schema"></a>Schéma

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

::: moniker range=">= aspnetcore-2.2"

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml

::: moniker-end
**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

::: moniker range=">= aspnetcore-2.2"

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml

::: moniker-end

### <a name="configuration"></a>Configuration

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * Visual Studio : {RACINE DE L’APPLICATION}\\.vs\config\applicationHost.config
   
   * *iisexpress.exe* CLI : %PROFILUTILISATEUR%\Documents\IISExpress\config\applicationhost.config

Vous trouverez les fichiers en recherchant *aspnetcore* dans le fichier *applicationHost.config*.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:host-and-deploy/iis/index>
* [Dépôt GitHub du module ASP.NET Core (source de référence)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
