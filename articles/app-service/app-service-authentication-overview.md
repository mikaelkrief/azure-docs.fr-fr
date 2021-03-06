---
title: Authentification et autorisation dans Azure App Service | Microsoft Docs
description: Référence et présentation conceptuelles de la fonctionnalité d’authentification/autorisation pour Azure App Service.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: c180dcf5d769245f3fa2485ccee2cbc18ecf5f67
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33763490"
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Authentification et autorisation dans Azure App Service

Azure App Service offre une prise en charge intégrée de l’authentification et de l’autorisation, qui vous permet de connecter les utilisateurs et d’accéder aux données sans avoir à écrire beaucoup de code dans votre application web, votre API et votre back end mobile, ainsi que dans [Azure Functions](../azure-functions/functions-overview.md). Cet article explique comment App Service contribue à simplifier l’authentification et l’autorisation de votre application. 

Pour mettre en place un système sécurisé d’authentification et d’autorisation, il faut avoir une connaissance approfondie de la sécurité, notamment de la fédération, du chiffrement, de la gestion des [jetons web JSON (JWT)](https://wikipedia.org/wiki/JSON_Web_Token), des [types d’autorisation](https://oauth.net/2/grant-types/), etc. App Service propose ces utilitaires pour vous permettre de consacrer davantage de temps et d’énergie à offrir de la valeur ajoutée à votre client.

> [!NOTE]
> Il n’est pas obligatoire d’utiliser App Service pour l’authentification et l’autorisation. Plusieurs infrastructures web sont fournies avec des fonctionnalités de sécurité ; vous pouvez les utiliser si vous le souhaitez. Si vous avez besoin de plus de flexibilité que n’en offre App Service, vous pouvez également écrire vos propres utilitaires.  
>

Pour plus d’informations sur les applications mobiles natives en particulier, consultez la page [Authentification et autorisation des utilisateurs pour les applications mobiles avec Azure App Service](../app-service-mobile/app-service-mobile-auth.md).

## <a name="how-it-works"></a>Fonctionnement

Le module d’authentification et d’autorisation s’exécute dans le même bac à sable que le code de l’application. Lorsqu’il est activé, chaque requête HTTP entrante le traverse avant d’être géré par le code de l’application.

![](media/app-service-authentication-overview/architecture.png)

Ce module gère plusieurs choses pour votre application :

- authentification des utilisateurs avec le fournisseur spécifié ;
- validation, stockage et actualisation des jetons ;
- gestion de la session authentifiée ;
- injection d’informations d’identité dans les en-têtes de demande.

Le module, configuré à l’aide des paramètres de l’application, s’exécute de façon distincte du code de l’application. Aucun Kit de développement logiciel (SDK), aucun langage spécifique ni aucune modification du code de l’application ne sont nécessaires. 

### <a name="user-claims"></a>Revendications d’utilisateur

Pour toutes les infrastructures de langage, App Service rend les revendications de l’utilisateur accessibles à votre code en les insérant dans les en-têtes de demande. Dans le cas des applications ASP.NET 4.6, App Service remplit [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) avec les revendications de l’utilisateur authentifié, ce qui vous permet de suivre le modèle de code .NET standard, attribut `[Authorize]` compris. De même, pour les applications PHP, App Service remplit la variable `_SERVER['REMOTE_USER']`.

Avec [Azure Functions](../azure-functions/functions-overview.md), `ClaimsPrincipal.Current` n’est pas alimenté pour le code .NET, mais les revendications d’utilisateur se trouvent toujours dans les en-têtes de demande.

Pour plus d’informations, consultez la section [Revendications d’utilisateurs d’accès](app-service-authentication-how-to.md#access-user-claims).

### <a name="token-store"></a>Magasin de jetons

App Service fournit un magasin de jetons intégré : il s’agit d’un référentiel de jetons associés aux utilisateurs de vos applications web, API ou applications mobiles natives. Dès que l’authentification est activée avec un fournisseur, l’application a immédiatement accès à ce magasin de jetons. Si le code de votre application doit accéder à des données de ces fournisseurs au nom de l’utilisateur, notamment : 

- publier sur le fil d’actualité Facebook de l’utilisateur authentifié ;
- lire les données d’entreprise de l’utilisateur à partir de l’API Graph Azure Active Directory ou même de Microsoft Graph.

Les jetons d’ID, jetons d’accès et jetons d’actualisation mis en cache pour la session authentifiée ne sont accessibles qu’à l’utilisateur associé.  

Il est en général nécessaire d’écrire du code pour recueillir, stocker et actualiser ces jetons dans votre application. Avec le magasin de jetons, il suffit de [récupérer les jetons](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) en temps utile et de [demander à App Service de les actualiser](app-service-authentication-how-to.md#refresh-access-tokens) lorsqu’ils ne sont plus valides. 

Si vous n’avez pas besoin de travailler avec des jetons dans votre application, vous pouvez désactiver le magasin de jetons.

### <a name="logging-and-tracing"></a>Journalisation et suivi

Si vous [activez la journalisation des applications](web-sites-enable-diagnostic-log.md), les traces de l’authentification et de l’autorisation apparaîtront directement dans les fichiers journaux. Si une erreur d’authentification inattendue se produit, vous trouverez facilement tous les détails dans les journaux existants. Si vous activez le [suivi des échecs des demandes](web-sites-enable-diagnostic-log.md), vous saurez exactement quel rôle le module d’authentification et d’autorisation a pu jouer dans l’échec d’une demande. Dans les journaux de suivi, recherchez les références à un module nommé `EasyAuthModule_32/64`. 

## <a name="identity-providers"></a>Fournisseurs d’identité

App Service utilise [l’identité fédérée](https://en.wikipedia.org/wiki/Federated_identity), dans laquelle un fournisseur d’identité tiers gère automatiquement l’identité des utilisateurs et le flux d’authentification. Cinq fournisseurs d’identité sont disponibles par défaut : 

| Fournisseur | Point de terminaison de connexion |
| - | - |
| [Azure Active Directory](../active-directory/active-directory-whatis.md) | `/.auth/login/aad` |
| [Compte Microsoft](../active-directory/develop/active-directory-appmodel-v2-overview.md) | `/.auth/login/microsoft` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/+/web/api/rest/oauth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/docs/basics/authentication) | `/.auth/login/twitter` |

Lorsque l’authentification et l’autorisation sont activées avec un de ces fournisseurs, son point de terminaison de connexion est accessible à des fins d’authentification de l’utilisateur et de validation des jetons d’authentification provenant du fournisseur. Vous pouvez proposer à vos utilisateurs toutes les options de connexion que vous souhaitez parmi celles-ci, en toute simplicité. Vous avez également la possibilité d’intégrer un autre fournisseur d’identité ou [votre propre solution d’identité personnalisée][custom-auth].

## <a name="authentication-flow"></a>Flux d’authentification

Le flux d’authentification est identique pour tous les fournisseurs, mais il diffère selon que vous souhaitez ou non ouvrir une session avec le Kit de développement logiciel (SDK) du fournisseur :

- Sans le Kit SDK du fournisseur : l’application délègue l’authentification fédérée à App Service. C’est généralement le cas avec les applications de navigateur, qui peuvent présenter la page de connexion du fournisseur à l’utilisateur. C’est le code du serveur qui gère le processus de connexion, c’est pourquoi il est également appelé _flux dirigé vers le serveur_ ou _flux serveur_. Ce cas s’applique aux applications web. Il concerne également les applications natives qui connectent les utilisateurs à l’aide du Kit SDK client Mobile Apps, car celui-ci ouvre un affichage web pour connecter les utilisateurs avec l’authentification App Service. 
- Avec le Kit SDK du fournisseur : l’application connecte l’utilisateur manuellement et soumet ensuite le jeton d’authentification à App Service pour validation. C’est généralement le cas avec les applications sans navigateur, qui ne peuvent pas présenter la page de connexion du fournisseur à l’utilisateur. C’est le code de l’application qui gère le processus de connexion, c’est pourquoi il est également appelé _flux dirigé vers le client_ ou _flux client_. Ce cas s’applique aux API REST, à [Azure Functions](../azure-functions/functions-overview.md) et aux clients de navigateur JavaScript, ainsi qu’aux applications web nécessitant plus de souplesse dans le processus de connexion. Il concerne également les applications mobiles natives qui connectent les utilisateurs à l’aide du Kit SDK du fournisseur.

> [!NOTE]
> Les appels provenant d’une application de navigateur approuvée dans App Service appellent une autre API REST dans App Service ou [Azure Functions](../azure-functions/functions-overview.md) ; l’authentification est possible suivant le flux dirigé vers le serveur. Pour plus d’informations, consultez la page [Authentifier les utilisateurs avec Azure App Service]().
>

Le tableau ci-dessous montre les étapes du flux d’authentification.

| Étape | Sans le Kit SDK du fournisseur | Avec le Kit SDK du fournisseur |
| - | - | - |
| 1. Connexion de l’utilisateur | Redirige le client vers `/.auth/login/<provider>`. | Le code client connecte directement l’utilisateur avec le Kit SDK du fournisseur et reçoit un jeton d’authentification. Pour plus d’informations, consultez la documentation du fournisseur. |
| 2. Post-authentification | Le fournisseur redirige le client vers `/.auth/login/<provider>/callback`. | Le code client publie un jeton du fournisseur vers `/.auth/login/<provider>` pour validation. |
| 3. Établissement de la session authentifiée | App Service ajoute un cookie authentifié à la réponse. | App Service retourne son propre jeton d’authentification au code client. |
| 4. Traitement du contenu authentifié | Le client inclut le cookie d’authentification dans les demandes suivantes (gérées automatiquement par le navigateur). | Le code client présente le jeton d’authentification dans l’en-tête `X-ZUMO-AUTH` (géré automatiquement par les Kits SDK clients Mobile Apps). |

Dans le cas des navigateurs clients, App Service peut diriger automatiquement tous les utilisateurs non authentifiés vers `/.auth/login/<provider>`. Vous pouvez également présenter aux utilisateurs un ou plusieurs liens `/.auth/login/<provider>` pour leur permettre de se connecter à votre application à l’aide du fournisseur de leur choix.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Comportement d’autorisation

Sur le [Portail Azure](https://portal.azure.com), vous pouvez configurer l’autorisation App Service avec différents comportements.

![](media/app-service-authentication-overview/authorization-flow.png)

Les titres suivants décrivent les options possibles.

### <a name="allow-all-requests-default"></a>Autoriser toutes les demandes (par défaut)

L’authentification et l’autorisation ne sont pas gérées par App Service (désactivé). 

Choisissez cette option si vous n’avez pas besoin d’authentification ni d’autorisation, ou que vous souhaitez écrire votre propre code d’authentification et d’autorisation.

### <a name="allow-only-authenticated-requests"></a>Autoriser uniquement les demandes authentifiées

L’option est **Se connecter avec \<fournisseur >**. App Service redirige toutes les demandes anonymes vers `/.auth/login/<provider>` pour le fournisseur choisi. Si la demande anonyme provient d’une application mobile native, la réponse retournée est `HTTP 401 Unauthorized`.

Cette option évite d’avoir à écrire du code d’authentification dans l’application. Une autorisation plus fine, par exemple propre au rôle, peut être gérée en examinant les revendications de l’utilisateur (consultez la section [Accéder aux revendications utilisateur](app-service-authentication-how-to.md#access-user-claims)).

### <a name="allow-all-requests-but-validate-authenticated-requests"></a>Autoriser toutes les demandes, mais valider les demandes authentifiées

L’option est **Autoriser les requêtes anonymes**. Cette option active l’authentification et l’autorisation dans App Service, mais délègue les décisions d’autorisation au code de l’application. Dans le cas des demandes authentifiées, App Service transmet également les informations d’authentification dans les en-têtes HTTP. 

Cette option assure un traitement plus souple des requêtes anonymes. Par exemple, il permet de [présenter plusieurs options de connexion](app-service-authentication-how-to.md#configure-multiple-sign-in-options) aux utilisateurs. Elle implique toutefois d’écrire du code. 

## <a name="more-resources"></a>Autres ressources

[Didacticiel : authentifier et autoriser les utilisateurs de bout en bout dans Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)  
[Didacticiel : authentifier et autoriser les utilisateurs de bout en bout dans Azure App Service pour Linux](containers/tutorial-auth-aad.md)  
[Personnaliser l’authentification et l’autorisation dans App Service](app-service-authentication-how-to.md)

Guides pratiques propres à chaque fournisseur :

* [Configurer votre application App Service pour utiliser la connexion Azure Active Directory][AAD]
* [Comment configurer votre application App Service de manière à utiliser la connexion via Facebook][Facebook]
* [Comment configurer votre application App Service de manière à utiliser la connexion via Google][Google]
* [Comment configurer votre application App Service pour utiliser une connexion par compte Microsoft][MSA]
* [Comment configurer votre application App Service de manière à utiliser la connexion via Twitter][Twitter]
* [Guide pratique pour utiliser l’authentification personnalisée dans une application][custom-auth]

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
