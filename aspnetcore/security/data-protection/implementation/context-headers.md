---
title: En-têtes de contexte dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation des en-têtes de contexte de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 2343e59898c024eba420390d7fb0bce2fc82a895
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033216"
---
# <a name="context-headers-in-aspnet-core"></a>En-têtes de contexte dans ASP.NET Core

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>En arrière-plan et la théorie

Dans le système de protection de données, une « clé » signifie qu’un objet qui peut fournir authentifié des services de chiffrement. Chaque clé est identifiée par un id unique (GUID), et elle comporte des informations algorithmiques et matériels entropic. Il est prévu que chaque clé transporter entropie unique, mais le système ne peut pas appliquer qui, et nous avons également besoin pour prendre en compte pour les développeurs qui peuvent changer le key ring manuellement en modifiant les informations algorithmiques d’une clé existante dans le key ring. Pour atteindre nos exigences de sécurité étant données ces cas, le système de protection de données a un concept de [agilité de chiffrement](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), ce qui permet en toute sécurité à l’aide d’une seule valeur entropic entre plusieurs algorithmes de chiffrement.

La plupart des systèmes qui prennent en charge de la souplesse cryptographique ce faire, y compris des informations d’identification sur l’algorithme à l’intérieur de la charge utile. OID de l’algorithme est généralement un bon candidat pour cela. Toutefois, l’un des problèmes que nous ayons rencontré sont qu’il existe plusieurs manières de spécifier le même algorithme : « AES » (CNG) et le géré Aes, AesManaged, AesCryptoServiceProvider, AesCng et RijndaelManaged (étant donné les paramètres spécifiques) classes sont toutes en fait la même chose, et nous devons conservent un mappage de tous ces éléments pour le bon OID. Si un développeur souhaitions offrir à un algorithme personnalisé (ou même une autre implémentation de AES), ils devaient dites-nous son OID. Cette étape d’inscription supplémentaires rend la configuration du système particulièrement difficile.

Retours en arrière, nous avons décidé que nous étions approche consistant à partir de la mauvaise direction. Un OID vous indique ce que l’algorithme, mais en réalité importent à ce sujet. Si nous devons utiliser une seule valeur entropic en toute sécurité dans les deux algorithmes différents, il n’est pas nécessaire pour nous permettre de savoir quels sont les algorithmes. Ce qui nous importe réellement est leur comportement. N’importe quel algorithme de chiffrement par bloc symétriques décent est également une permutation pseudo-aléatoire forte (PRP) : corriger les entrées (clé, en texte clair du vecteur d’initialisation, le mode de chaînage) et la sortie de texte chiffré sera différente à partir de n’importe quel autre chiffrement de blocs symétrique avec la probabilité est surchargé algorithme étant donné les mêmes entrées. De même, n’importe quelle fonction de hachage à clé correcte est également une forte fonction pseudo-aléatoire (PRF), et étant donné un ensemble fixe d’entrée sa sortie sera extrêmement différente à partir de toute autre fonction de hachage à clé.

Nous utilisons ce concept de fort PRPs et PRF pour créer un en-tête de contexte. Cet en-tête de contexte agit essentiellement comme une empreinte numérique stable sur les algorithmes en cours d’utilisation pour toute opération donnée, et il fournit l’agilité de chiffrement requise par le système de protection des données. Cet en-tête est reproductible et est utilisé ultérieurement dans le cadre de la [processus de dérivation de sous-clé](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). Il existe deux façons de générer l’en-tête de contexte selon les modes de fonctionnement des algorithmes sous-jacents.

## <a name="cbc-mode-encryption--hmac-authentication"></a>Le chiffrement en mode CBC + authentification de HMAC

<a name="data-protection-implementation-context-headers-cbc-components"></a>

L’en-tête de contexte se compose des éléments suivants :

* [16 bits] La valeur 00 00, ce qui est un marqueur de ce qui signifie « chiffrement CBC + authentification de HMAC ».

* [32 bits] La longueur de clé (en octets, big-endian) de l’algorithme de chiffrement par bloc symétriques.

* [32 bits] La taille de bloc (en octets, big-endian) de l’algorithme de chiffrement par bloc symétriques.

* [32 bits] La longueur de clé (en octets, big-endian) de l’algorithme HMAC. (Actuellement la taille de la clé correspond toujours à la taille de synthèse.)

* [32 bits] La taille de la synthèse (en octets, big-endian) de l’algorithme HMAC.

* EncCBC (K_E, IV, « »), qui est la sortie de l’algorithme de chiffrement symétrique étant donné une entrée de chaîne vide et où le vecteur d’initialisation est un vecteur de zéros. La construction de K_E est décrite ci-dessous.

* MAC (K_H, « »), qui est la sortie de l’algorithme HMAC étant donné une entrée de chaîne vide. La construction de K_H est décrite ci-dessous.

Dans l’idéal, nous pourrions passer des vecteurs de zéros pour K_E et K_H. Toutefois, nous souhaitons éviter les situations où l’algorithme sous-jacent vérifie l’existence de clés faibles avant d’effectuer des opérations (notamment DES et 3DES), ce qui empêche à l’aide d’un modèle simple ou répétable comme un vecteur de zéros.

Au lieu de cela, nous utilisons NIST SP800-108 KDF en Mode de compteur (consultez [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) avec une clé de longueur nulle, étiquette, contexte et des HMACSHA512 comme le PRF sous-jacent. Nous dérivons | K_E | + | K_H | octets de sortie, puis décomposer le résultat K_E et K_H eux-mêmes. Point de vue mathématique, cela est représenté comme suit.

(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, clé = « », étiquette = « », contexte = « »)

### <a name="example-aes-192-cbc--hmacsha256"></a>Exemple : AES-192-CBC + HMACSHA256

Par exemple, prenons le cas où l’algorithme de chiffrement symétrique est AES-CBC-192 et l’algorithme de validation est HMACSHA256. Le système génère l’en-tête de contexte en procédant comme suit.

Commençons (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, clé = « », étiquette = « », contexte = « »), où | K_E | = 192 bits et | K_H | = 256 bits par les algorithmes spécifiés. Cela conduit à K_E = 5BB6... 21DD et K_H = A04A... 00A9 dans l’exemple ci-dessous :

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

Ensuite, calcul Enc_CBC (K_E, IV, « ») pour AES-192-CBC, étant donné le vecteur d’initialisation = 0 * et K_E comme indiqué ci-dessus.

résultat : = F474B1872B3B53E4721DE19C0841DB6F

Ensuite, calcul MAC (K_H, « ») pour HMACSHA256 donné K_H comme ci-dessus.

résultat : = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

Cela génère l’en-tête de contexte complet ci-dessous :

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Cet en-tête de contexte est l’empreinte numérique de la paire d’algorithme de chiffrement authentifié (chiffrement CBC AES-192 + HMACSHA256 validation). Les composants, comme décrit [ci-dessus](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) sont :

* le marqueur (00 00)

* la longueur de clé de chiffrement de bloc (00 00 00 18)

* la taille de bloc de chiffrement par bloc (00 00 00 10)

* la longueur de clé HMAC (00 00 00 20)

* la taille de digest HMAC (00 00 00 20)

* le chiffrement par blocs sortie PRP (F4 74 - DB 6F) et

* la sortie HMAC PRF (D4 79 - fin).

> [!NOTE]
> Le chiffrement en mode CBC + HMAC en-tête de contexte d’authentification est intégrée à la même façon, quel que soit l’indique si les implémentations d’algorithmes sont fournies par Windows CNG ou par des types managés de SymmetricAlgorithm et élément KeyedHashAlgorithm impossible. Ainsi, les applications qui s’exécutent sur différents systèmes d’exploitation générer de manière fiable le même en-tête de contexte même si les implémentations des algorithmes diffèrent entre les systèmes d’exploitation. (Dans la pratique, l’élément KeyedHashAlgorithm impossible ne doit être un code HMAC approprié. Il peut être n’importe quel type d’algorithme de hachage à clé.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Exemple : 3DES-192-CBC + HMACSHA1

Commençons (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, clé = « », étiquette = « », contexte = « »), où | K_E | = 192 bits et | K_H | = 160 bits par les algorithmes spécifiés. Cela conduit à K_E = A219... E2BB et K_H = DC4A... B464 dans l’exemple ci-dessous :

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

Ensuite, calcul Enc_CBC (K_E, IV, « ») pour 3DES-192-CBC, étant donné le vecteur d’initialisation = 0 * et K_E comme indiqué ci-dessus.

résultat : = ABB100F81E53E10E

Ensuite, calcul MAC (K_H, « ») pour HMACSHA1 donné K_H comme ci-dessus.

résultat : = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Cela génère l’en-tête de contexte complet qui est une empreinte numérique des authentifiés paire d’algorithme de chiffrement (chiffrement 3DES-192-CBC + HMACSHA1 validation), illustré ci-dessous :

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

Les composants se décomposent comme suit :

* le marqueur (00 00)

* la longueur de clé de chiffrement de bloc (00 00 00 18)

* la taille de bloc de chiffrement par bloc (00 00 00 08)

* la longueur de clé HMAC (00 00 00 14)

* la taille de digest HMAC (00 00 00 14)

* le chiffrement par blocs sortie PRP (B1 AB - E1 0E) et

* la sortie HMAC PRF (EB 76 - fin).

## <a name="galoiscounter-mode-encryption--authentication"></a>Le chiffrement en Mode de compteur/GALOIS + authentification

L’en-tête de contexte se compose des éléments suivants :

* [16 bits] La valeur 00 01, qui est un marqueur de ce qui signifie « chiffrement de GCM + authentification ».

* [32 bits] La longueur de clé (en octets, big-endian) de l’algorithme de chiffrement par bloc symétriques.

* [32 bits] Taille de valeur à usage unique (en octets, big-endian) utilisée pendant les opérations de chiffrement authentifié. (Pour notre système, ce problème est résolu à la taille de la valeur à usage unique = 96 bits.)

* [32 bits] La taille de bloc (en octets, big-endian) de l’algorithme de chiffrement par bloc symétriques. (Pour GCM, ce problème est résolu à la taille de bloc = 128 bits.)

* [32 bits] Authentification balise taille (en octets, big-endian) générée par la fonction de chiffrement authentifié. (Pour notre système, ce problème est résolu à la taille de balise = 128 bits.)

* [128 bits] La balise de Enc_GCM (K_E, la valeur à usage unique, « »), qui est la sortie de l’algorithme de chiffrement symétrique étant donné une entrée de chaîne vide et où la valeur à usage unique est un vecteur de zéros de 96 bits.

K_E est dérivée à l’aide du même mécanisme que dans le chiffrement CBC + d’un scénario d’authentification HMAC. Toutefois, puisqu’il n’existe aucun K_H en jeu ici, nous ont fondamentalement | K_H | = 0, et l’algorithme est réduit à la sous la forme.

K_E = SP800_108_CTR (prf = HMACSHA512, clé = « », étiquette = « », contexte = « »)

### <a name="example-aes-256-gcm"></a>Exemple : AES-256-GCM

Tout d’abord, laisser K_E = SP800_108_CTR (prf = HMACSHA512, clé = « », étiquette = « », contexte = « »), où | K_E | = 256 bits.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

Ensuite, la balise d’authentification de Enc_GCM de calcul (K_E, la valeur à usage unique, « ») pour AES-256-GCM, étant donné la valeur à usage unique = 096 et K_E comme indiqué ci-dessus.

résultat : = E7DCCE66DF855A323A6BB7BD7A59BE45

Cela génère l’en-tête de contexte complet ci-dessous :

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

Les composants se décomposent comme suit :

   * le marqueur (00 01)

   * la longueur de clé de chiffrement de bloc (00 00 00 20)

   * la taille de la valeur à usage unique (00 00 00 0 C)

   * la taille de bloc de chiffrement par bloc (00 00 00 10)

   * la taille de balise d’authentification (00 00 00 10) et

   * la balise d’authentification de l’exécution du chiffrement par blocs (contrôleur de domaine E7 - fin).
