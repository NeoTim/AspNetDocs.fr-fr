---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: ASP.NET Aperçu de MVC (fr) Microsoft Docs
author: rick-anderson
description: Renseignez-vous sur les différences entre ASP.NET’application MVC et les applications ASP.NET Web Forms. Découvrez comment décider quand construire une application MVC ASP.NET.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 84810f168faa2e79a65ff3fb75e3654828bdb58f
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541011"
---
# <a name="aspnet-mvc-overview"></a>Vue d’ensemble d’ASP.NET MVC

par [Microsoft](https://github.com/microsoft)

> Renseignez-vous sur les différences entre ASP.NET’application MVC et les applications ASP.NET Web Forms. Découvrez comment décider quand construire une application MVC ASP.NET.

Le modèle architectural MVC (Model-View-Controller) sépare une application en trois composants principaux : le modèle, la vue et le contrôleur. Le cadre MVC ASP.NET offre une alternative au modèle ASP.NET Web Forms pour la création d’applications Web basées sur MVC. ASP.NET MVC est une infrastructure de présentation simple et facilement testable, qui (comme celle des applications utilisant des Web Forms) est intégrée aux fonctionnalités ASP.NET existantes, telles que les pages maîtres et l'authentification basée sur l'appartenance. Le cadre MVC est défini dans l’espace de nom **System.Web.Mvc** et est une partie fondamentale et soutenue de l’espace de nom **System.Web.**   
  
MVC est un modèle de conception standard qui est connu par de nombreux développeurs. Certains types d'applications Web tireront parti de l'infrastructure MVC. D'autres continueront à utiliser le modèle d'application ASP.NET traditionnel qui est basé sur les Web Forms et les publications (postbacks). D'autres encore combineront les deux approches, l'une n'excluant pas l'autre.   
  
L'infrastructure MVC inclut les composants suivants :

[![Invoquer une action de contrôleur qui s’attend à une valeur de paramètre](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figure 01**: Invoquer une action de contrôleur qui s’attend à une valeur de paramètre ([Cliquez pour voir l’image grandeur nature](asp-net-mvc-overview/_static/image2.png))

- **Modèles**. Les objets modèles sont les parties de l’application qui implémentent la logique du domaine des données de l application. Souvent, ils récupèrent l'état du modèle et le stockent dans une base de données. Par exemple, un objet produit peut récupérer des informations d’une base de données, l’utiliser, puis écrire des informations mises à jour vers une table de produits dans SQL Server.

Dans les petites applications, le modèle est souvent une séparation conceptuelle plutôt qu'une séparation physique. Par exemple, si l’application ne lit qu’un ensemble de données et l’envoie à la vue, l’application n’a pas de couche de modèle physique et de classes associées. Dans ce cas, l’ensemble de données prend le rôle d’un objet modèle.

- **Vues**. Les vues sont les composants qui affichent l interface utilisateur de l’application (interface utilisateur). En général, cette interface utilisateur est créée à partir des données du modèle. Un exemple serait une vue de modification d’une table de produits qui affiche des boîtes de texte, des listes de dépôt, et des cases de cocher en fonction de l’état actuel d’un objet Produits.

- **Contrôleurs**. Les contrôleurs sont les composants qui gèrent les interventions de l'utilisateur, exploitent le modèle et finalement sélectionnent une vue permettant de restituer l'interface utilisateur. Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond. Par exemple, le contrôleur gère les valeurs de la chaîne de requêtes et transmet ces valeurs au modèle, qui à son tour interroge la base de données en utilisant les valeurs.

Le modèle MVC vous aide à créer des applications qui séparent les différents aspects de l'application (logique d'entrée, logique métier et logique de l'interface utilisateur) en assurant un couplage lâche entre ces éléments. Le modèle spécifie l'emplacement où chaque genre de logique doit figurer dans l'application. La logique de l’interface utilisateur appartient à la vue. La logique d’entrée appartient au contrôleur. La logique métier appartient au modèle. Cette séparation vous aide à gérer la complexité lorsque vous générez une application car elle vous permet de vous concentrer sur un aspect de l'implémentation à la fois. Par exemple, vous pouvez vous concentrer sur la vue sans dépendre de la logique métier.   
  
Le modèle MVC permet non seulement de gérer la complexité, mais également de simplifier le processus de test comparativement à celui d'une application Web ASP.NET basée sur des Web Forms. Par exemple, dans une application Web ASP.NET basée sur des Web Forms, une même classe est utilisée pour afficher la sortie et répondre aux interventions de l'utilisateur. L'écriture de tests automatisés pour les applications ASP.NET basées sur des Web Forms peut s'avérer complexe, car pour tester une page individuelle, vous devez instancier la classe de la page, tous ses contrôles enfants et les autres classes dépendantes dans l'application. Étant donné que de nombreuses classes sont instanciées pour exécuter la page, il peut s'avérer difficile d'écrire des tests centrés exclusivement sur des parties individuelles de l'application. Par conséquent, les tests des applications ASP.NET basées sur des Web Forms peuvent être plus complexes à implémenter que les tests d'une application MVC. En outre, les tests d'une application ASP.NET basée sur des Web Forms requièrent un serveur Web. L'infrastructure MVC dissocie les composants et utilise de façon intensive les interfaces, ce qui permet de tester des composants individuels en les isolant du reste de l'infrastructure.   
  
Le couplage lâche entre les trois principaux composants d'une application MVC favorise également le développement en parallèle. Par exemple, un développeur peut travailler sur la vue, un deuxième développeur peut travailler sur la logique du contrôleur, et un troisième développeur peut se concentrer sur la logique d’entreprise dans le modèle.

## <a name="deciding-when-to-create-an-mvc-application"></a>Décider quand créer une application MVC

Vous devez déterminer avec soin s'il convient d'implémenter une application Web à l'aide de l'infrastructure ASP.NET MVC ou du modèle Web Forms ASP.NET. L'infrastructure MVC ne remplace pas le modèle Web Forms ; vous pouvez utiliser l'une ou l'autre des infrastructures pour les applications Web. (Si certaines de vos applications sont basées sur des Web Forms, elles continuent à fonctionner exactement comme elles l'ont toujours fait.)   
  
Avant de décider d'utiliser l'infrastructure MVC ou le modèle Web Forms pour un site Web spécifique, évaluez les avantages de chaque approche.

### <a name="advantages-of-an-mvc-based-web-application"></a>Avantages d'une application Web basée sur MVC

L'infrastructure ASP.NET MVC offre les avantages suivants :

- Elle permet de gérer plus facilement la complexité en décomposant une application en modèle, vue et contrôleur.
- Elle n'utilise pas l'état d'affichage ni les formulaires serveur. L'infrastructure MVC s'avère par conséquent idéale pour les développeurs souhaitant contrôler totalement le comportement d'une application.
- Elle utilise un modèle de contrôleur frontal qui traite les requêtes de l'application Web par le biais d'un contrôleur unique. De cette façon, vous pouvez concevoir une application prenant en charge une infrastructure de routage complète. Pour plus d’informations, voir [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Contrôleur avant") sur le site Web msDN.
- Elle offre une meilleure prise en charge du développement axé sur des tests.
- Il fonctionne bien pour les applications Web qui sont prises en charge par de grandes équipes de développeurs et de concepteurs Web qui ont besoin d’un haut degré de contrôle sur le comportement de l’application.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Avantages d'une application Web basée sur des Web Forms

L'infrastructure basée sur des Web Forms offre les avantages suivants :

- Elle prend en charge un modèle d'événement permettant la conservation de l'état via HTTP, mécanisme qui s'avère particulièrement utile pour le développement d'applications Web métier. L'application basée sur des Web Forms fournit des dizaines d'événements pris en charge dans des centaines de contrôles serveur.
- Elle utilise un modèle de contrôleur de pages qui ajoute des fonctionnalités aux pages individuelles. Pour plus d’informations, consultez [le contrôleur de page](https://go.microsoft.com/fwlink/?LinkId=106359 "Contrôleur de page") sur le site Web msDN.
- Il utilise des formulaires d’état de vue ou de serveur, ce qui peut faciliter la gestion des informations de l’État.
- Elle est parfaitement adaptée aux petites équipes de concepteurs et de développeurs Web qui souhaitent tirer parti des nombreux composants disponibles pour le développement rapide d'applications.
- En général, il est moins complexe pour le développement d’applications, parce que les composants (la classe **De page,** les contrôles, etc.) sont étroitement intégrés et nécessitent généralement moins de code que le modèle MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Fonctionnalités de l'infrastructure ASP.NET MVC

L'infrastructure ASP.NET MVC intègre les fonctionnalités suivantes :

- Séparation des tâches d’application (logique d’entrée, logique d’entreprise et logique d’interface utilisateur), testabilité et développement piloté par test (TDD) par défaut. Tous les contrats de base de l'infrastructure MVC sont basés sur des interfaces et peuvent être testés à l'aide d'objets fictifs qui simulent le comportement d'objets réels de l'application. Les tests unitaires de l'application sont à la fois rapides et flexibles étant donné que vous pouvez les effectuer sans avoir à exécuter les contrôleurs dans un processus ASP.NET. Vous pouvez utiliser toute infrastructure de test unitaire compatible avec .NET Framework.
- Infrastructure extensible et enfichable. Les composants de l'infrastructure ASP.NET MVC sont conçus de façon à pouvoir être facilement remplacés ou personnalisés. Vous pouvez incorporer vos propres moteur d'affichage, stratégie de routage des URL et sérialisation des paramètres de méthode d'action, ainsi que d'autres composants. L'infrastructure ASP.NET MVC prend en charge également l'utilisation des modèles de conteneur Injection de dépendances et Inversion de contrôle. DI vous permet d’injecter des objets dans une classe, au lieu de compter sur la classe pour créer l’objet lui-même. L'inversion de contrôle spécifie que si un objet requiert un autre objet, le premier objet doit obtenir le deuxième à partir d'une source extérieure, telle qu'un fichier de configuration. Ce processus facilite les tests.
- Un puissant composant de cartographie d’URL qui vous permet de créer des applications qui ont des URL compréhensibles et consultables. Les URL ne doivent pas nécessairement inclure des extensions de nom de fichier et sont conçues pour prendre en charge des modèles de dénomination d'URL adaptés à l'optimisation des moteurs de recherche (SEO, Search Engine Optimization) et à l'adressage au format REST (Representational State Transfer).
- Prise en charge du balisage des fichiers de balisage de page ASP.NET (fichiers .aspx), de contrôle utilisateur (fichiers .ascx) et de page maître (fichiers .master) existants en tant que modèles de vue. Vous pouvez utiliser les fonctionnalités ASP.NET existantes avec le cadre MVC ASP.NET, tels que&lt;les&gt;pages maîtresses imbriquées, les expressions en ligne (% , les contrôles déclaratifs du serveur, les modèles, la liaison de données, la localisation, etc.
- Prise en charge des fonctionnalités ASP.NET existantes. ASP.NET MVC vous permet d'utiliser des fonctionnalités telles que l'authentification par formulaire et l'authentification Windows, l'autorisation d'URL, l'appartenance et les rôles, la mise en cache de sortie et de données, la gestion d'état de session et de profil, le contrôle d'état, le système de configuration et l'architecture du fournisseur.
