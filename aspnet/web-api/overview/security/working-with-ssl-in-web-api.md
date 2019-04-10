---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Utilisation de SSL dans l’API Web | Microsoft Docs
author: MikeWasson
description: Montre comment utiliser le protocole SSL avec l’API Web ASP.NET, y compris à l’aide de certificats client SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59386150"
---
# <a name="working-with-ssl-in-web-api"></a>Utilisation de SSL dans l’API Web

par [Mike Wasson](https://github.com/MikeWasson)

Plusieurs schémas d’authentification ne sont pas sécurisés sur le HTTP standard. En particulier, l’authentification de base et l’authentification par formulaire envoient les informations d’identification non chiffrées. Pour être sécurisés, ces schémas d’authentification *doit* utiliser SSL. En outre, les certificats clients SSL peuvent être utilisés pour authentifier les clients.

## <a name="enabling-ssl-on-the-server"></a>L’activation de SSL sur le serveur

Pour configurer SSL dans IIS 7 ou version ultérieure :

- Créer ou obtenir un certificat. Pour le test, vous pouvez créer un certificat auto-signé.
- Ajouter une liaison HTTPS.

Pour plus d’informations, consultez [comment la configuration de SSL sur IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Pour le test local, vous pouvez activer SSL dans IIS Express à partir de Visual Studio. Dans la fenêtre Propriétés, définissez **SSL activé** à **True**. Notez la valeur de **URL SSL**; Utilisez cette URL pour le test des connexions HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>L’application de SSL dans un contrôleur d’API Web

Si vous avez une adresse HTTPS et une liaison HTTP, les clients peuvent toujours utiliser HTTP pour accéder au site. Vous pouvez autoriser certaines ressources soient disponibles via HTTP, tandis que les autres ressources exiger le protocole SSL. Dans ce cas, utilisez un filtre d’action pour exiger SSL pour les ressources protégées. Le code suivant montre un filtre de l’authentification d’API Web qui recherche SSL :

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Ajoutez ce filtre à toutes les actions API Web nécessitant SSL :

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificats Client SSL

Le protocole SSL fournit l’authentification à l’aide de certificats d’Infrastructure à clé publique. Le serveur doit fournir un certificat qui authentifie le serveur au client. Il est moins courant pour le client fournisse un certificat au serveur, mais il s’agit d’une option pour authentifier les clients. Pour utiliser des certificats de client avec SSL, vous avez besoin d’un moyen de distribuer des certificats auto-signés pour vos utilisateurs. Pour de nombreux types d’application, il s’agit pas d’une expérience utilisateur optimale, mais dans certains environnements (par exemple, enterprise), il peut être possible.

| Avantages | Inconvénients |
| --- | --- |
| -Informations d’identification de certificat sont plus fortes que le nom d’utilisateur/mot de passe. -SSL fournit un canal sécurisé terminé, avec l’authentification, l’intégrité des messages et le chiffrement du message. | -Vous devez obtenir et gérer les certificats d’infrastructure à clé publique. -La plate-forme cliente doit prendre en charge les certificats client SSL. |

Pour configurer IIS pour accepter les certificats clients, ouvrez le Gestionnaire IIS et procédez comme suit :

1. Cliquez sur le nœud de site dans l’arborescence.
2. Double-cliquez sur le **paramètres SSL** fonctionnalité dans le volet central.
3. Sous **certificats clients**, sélectionnez une des options suivantes : 

    - **Accepter**: IIS accepte un certificat à partir du client, mais n’en requiert pas.
    - **Nécessitent**: Exiger un certificat client. (Pour activer cette option, vous devez également sélectionner « Exiger SSL »)

Vous pouvez également définir ces options dans le fichier ApplicationHost.config :

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

Le **SslNegotiateCert** indicateur signifie IIS accepte un certificat à partir du client, mais ne requiert pas (équivalent à l’option « Accepter » dans le Gestionnaire des services Internet). Pour demander un certificat, définissez le **SslRequireCert** indicateur. Pour le test, vous pouvez également définir ces options dans IIS Express, dans l’hôte d’application local. Fichier de configuration situé dans « Documents\IISExpress\config ».

### <a name="creating-a-client-certificate-for-testing"></a>Création d’un certificat Client pour le test

À des fins de test, vous pouvez utiliser [MakeCert.exe](/windows/desktop/SecCrypto/makecert) pour créer un certificat client. Commencez par créer une autorité racine de test :

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert vous invitera à entrer un mot de passe pour la clé privée.

Ensuite, ajoutez le certificat pour le test « Trusted Root Certification Authorities « du serveur stocker, comme suit :

1. Ouvrez la console MMC.
2. Sous **fichier**, sélectionnez **ajouter/supprimer un composant logiciel enfichable**.
3. Sélectionnez **compte d’ordinateur**.
4. Sélectionnez **ordinateur Local** et terminez l’Assistant.
5. Sous le volet de navigation, développez le nœud « Trusted Root Certification Authorities ».
6. Sur le **Action** menu, pointez sur **toutes les tâches**, puis cliquez sur **importation** pour démarrer l’Assistant Importation de certificat.
7. Recherchez le fichier de certificat, TempCA.cer.
8. Cliquez sur **Open**, puis cliquez sur **suivant** et terminez l’Assistant. (Vous devrez entrer à nouveau le mot de passe.)

Créez maintenant un certificat client qui est signé par le premier certificat :

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>À l’aide de certificats clients dans l’API Web

Sur le côté serveur, vous pouvez obtenir le certificat client en appelant [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) sur le message de demande. La méthode retourne null s’il n’existe aucun certificat client. Sinon, elle retourne un **X509Certificate2** instance. Cet objet permet d’obtenir des informations à partir du certificat, telles que l’émetteur et le sujet. Ensuite, vous pouvez utiliser ces informations pour l’authentification et/ou d’autorisation.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
