---
title: Limites de requête Azure Resource Manager | Microsoft Docs
description: Décrit comment utiliser la limitation avec des requêtes Azure Resource Manager lorsque les limites d’abonnement ont été atteintes.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2018
ms.author: tomfitz
ms.openlocfilehash: f3dcb0c5036b2cfc38ef2a6a16269a8697bbd9e6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358861"
---
# <a name="throttling-resource-manager-requests"></a>Limitation des requêtes de Resource Manager
Pour chaque abonnement et locataire, Resource Manager limite les requêtes de lecture à 15 000 par heure et les requêtes d’écriture à 1 200 par heure. Ces limites s’appliquent à chaque instance Azure Resource Manager. Chaque région Azure comporte plusieurs instances, et Azure Resource Manager est déployé dans toutes les régions Azure.  Par conséquent, dans la pratique, les limites sont effectivement beaucoup plus importantes que celles-ci, car les requêtes utilisateur sont généralement prises en charge par de nombreuses instances différentes.

Si votre application ou script atteint ces limites, vous devez limiter vos requêtes. Cet article vous montre comment déterminer les requêtes restantes dont vous disposez avant d’atteindre la limite et comment réagir si vous avez atteint la limite.

Lorsque vous atteignez la limite, vous recevez le code d’état HTTP **429 Trop de requêtes**.

L’étendue du nombre de requêtes est votre abonnement ou votre locataire. Si vous disposez de plusieurs applications simultanées envoyant des requêtes dans votre abonnement, les requêtes de ces applications sont combinées pour déterminer le nombre de requêtes restantes.

Les requêtes étendues à l’abonnement sont celles qui impliquent la transmission de votre ID d’abonnement, par exemple pour récupérer les groupes de ressources dans votre abonnement. Les requêtes étendues au locataire n’incluent pas votre ID d’abonnement, notamment pour la récupération des emplacements Azure valides.

## <a name="remaining-requests"></a>Requêtes restantes
Vous pouvez déterminer le nombre de requêtes restantes en examinant les en-têtes de réponse. Chaque demande contient des valeurs pour le nombre de requêtes de lecture et d’écriture restantes. Le tableau suivant décrit les en-têtes de réponse que vous pouvez examiner pour ces valeurs :

| En-tête de réponse | Description |
| --- | --- |
| x-ms-ratelimit-remaining-subscription-reads |Requêtes de lecture restantes étendues à l’abonnement. Cette valeur est renvoyée pour les opérations de lecture. |
| x-ms-ratelimit-remaining-subscription-writes |Requêtes d’écriture restantes étendues à l’abonnement. Cette valeur est renvoyée pour les opérations d’écriture. |
| x-ms-ratelimit-remaining-tenant-reads |Requêtes de lecture restantes étendues au locataire |
| x-ms-ratelimit-remaining-tenant-writes |Requêtes d’écriture restantes étendues au locataire |
| x-ms-ratelimit-remaining-subscription-resource-requests |Requêtes de type de ressource restantes étendues à l’abonnement.<br /><br />Cette valeur d’en-tête est retournée uniquement si un service a remplacé la limite par défaut. Resource Manager ajoute cette valeur au lieu des requêtes de lecture ou d’écriture de l’abonnement. |
| x-ms-ratelimit-remaining-subscription-resource-entities-read |Requêtes de collecte de type de ressource restantes étendues à l’abonnement.<br /><br />Cette valeur d’en-tête est retournée uniquement si un service a remplacé la limite par défaut. Cette valeur fournit le nombre de requêtes de collecte restantes (répertorier les ressources). |
| x-ms-ratelimit-remaining-tenant-resource-requests |Requêtes de type de ressource restantes étendues au locataire.<br /><br />Cet en-tête est ajouté uniquement pour les requêtes au niveau du locataire, et uniquement si un service a remplacé la limite par défaut. Resource Manager ajoute cette valeur au lieu des requêtes de lecture ou d’écriture du locataire. |
| x-ms-ratelimit-remaining-tenant-resource-entities-read |Requêtes de collecte de type de ressource restantes étendues au locataire.<br /><br />Cet en-tête est ajouté uniquement pour les requêtes au niveau du locataire, et uniquement si un service a remplacé la limite par défaut. |

## <a name="retrieving-the-header-values"></a>Récupération des valeurs d’en-tête
La récupération de ces valeurs d’en-tête dans votre code ou script est similaire à la récupération de n’importe quelle valeur d’en-tête. 

Par exemple, en **C#**, vous récupérez la valeur d’en-tête d’un objet **HttpWebResponse** nommé **response** avec le code suivant :

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

Dans **PowerShell**, vous récupérez la valeur de l’en-tête d’une opération Invoke-WebRequest.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Ou, si voulez voir les requêtes restantes pour le débogage, vous pouvez définir le paramètre **-Debug** sur votre applet de commande **PowerShell**.

```powershell
Get-AzureRmResourceGroup -Debug
```

Retourne un grand nombre de valeurs, notamment la valeur de réponse suivante :

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
```

Pour obtenir des limites d’écriture, utilisez une opération d’écriture : 

```powershell
New-AzureRmResourceGroup -Name myresourcegroup -Location westus -Debug
```

Renvoie un grand nombre de valeurs, notamment celles-ci :

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
Created

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-writes: 1199
```

Dans **l’interface de ligne de commande**, vous récupérez la valeur d’en-tête à l’aide de l’option plus détaillée.

```azurecli
az group list --verbose --debug
```

Renvoie un grand nombre de valeurs, notamment celles-ci :

```azurecli
msrest.http_logger : Response status: 200
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Content-Encoding': 'gzip'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'Vary': 'Accept-Encoding'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-reads': '14998'
```

Pour obtenir des limites d’écriture, utilisez une opération d’écriture : 

```azurecli
az group create -n myresourcegroup --location westus --verbose --debug
```

Renvoie un grand nombre de valeurs, notamment celles-ci :

```azurecli
msrest.http_logger : Response status: 201
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Length': '163'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-writes': '1199'
```

## <a name="waiting-before-sending-next-request"></a>Attente avant l’envoi de la requête suivante
Lorsque vous atteignez la limite de requêtes, Resource Manager renvoie le code d’état HTTP **429** et une valeur **Retry-After** dans l’en-tête. La valeur **Retry-After** spécifie le nombre de secondes pendant lesquelles votre application doit attendre (ou rester en veille) avant d’envoyer la requête suivante. Si vous envoyez une requête avant l’écoulement du temps d’attente avant une nouvelle tentative, votre requête n’est pas traitée et une autre valeur de nouvelle tentative est renvoyée.

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations sur les limites et les quotas, consultez [Abonnement Azure et limites, quotas et contraintes du service](../azure-subscription-service-limits.md).
* Pour en savoir plus sur la gestion des demandes REST asynchrones, consultez [Suivi des opérations asynchrones Azure](resource-manager-async-operations.md).
