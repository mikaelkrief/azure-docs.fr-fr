---
title: Configurer des sources de données dans Azure Log Analytics | Microsoft Docs
description: Les sources de données définissent les données que Log Analytics collectent auprès des agents et d'autres sources connectées.  Cet article décrit la façon dont Log Analytics utilise les sources de données, explique en détail comment les configurer, et fournit un résumé des différentes sources de données disponibles.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2018
ms.author: bwren
ms.openlocfilehash: 5201d02b4f70f964f39b4fe135e4715732b9741a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31792546"
---
# <a name="data-sources-in-log-analytics"></a>Sources de données dans Log Analytics
Log Analytics collecte les données à partir de vos sources connectées et les stocke dans votre espace de travail Log Analytics.  Les données collectées à partir de chacune de ces sources dépendent des sources de données que vous configurez.  Les données dans Log Analytics sont stockées sous forme d’ensemble d’enregistrements.  Chaque source de données crée des enregistrements d'un type particulier, chaque type ayant son propre ensemble de propriétés.

![Collecte de données Log Analytics](./media/log-analytics-data-sources/overview.png)

Les sources de données sont différentes des [solutions de gestion](log-analytics-add-solutions.md) qui collectent également des données à partir de sources connectées et créent des enregistrements dans Log Analytics.  En plus de la collecte de données, les solutions incluent généralement des vues et des recherches de journaux pour vous aider à analyser l’opération d’une application ou d’un service particulier.


## <a name="summary-of-data-sources"></a>Résumé des sources de données
Le tableau suivant répertorie les sources de données actuellement disponibles dans Log Analytics.  Chacune de ces sources comporte un lien vers un article distinct qui fournit des détails sur cette source de données.   Il fournit également des informations sur leur méthode et leur fréquence de collecte de données dans Log Analytics.  Vous pouvez utiliser les informations de cet article pour identifier les différentes solutions disponibles et comprendre leurs exigences en termes de flux de données et de connexion. Pour obtenir des explications sur les colonnes, consultez [Détails sur la collecte de données pour les solutions de gestion dans Azure](../monitoring/monitoring-solutions-inventory.md).


| Source de données | Plateforme | Microsoft Monitoring Agent | Agent Operations Manager | Stockage Azure | Operations Manager requis ? | Données de l’agent Operations Manager envoyées via un groupe d’administration | Fréquence de collecte |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Journaux personnalisés](log-analytics-data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | à l'arrivée |
| [Journaux personnalisés](log-analytics-data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | à l'arrivée |
| [Journaux IIS](log-analytics-data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |5 minutes |
| [Compteurs de performances](log-analytics-data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |comme prévu, minimum de 10 secondes |
| [Compteurs de performances](log-analytics-data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |comme prévu, minimum de 10 secondes |
| [Syslog](log-analytics-data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |depuis le stockage Azure : 10 minutes ; à partir de l’agent : à l’arrivée |
| [Journaux d’événements Windows](log-analytics-data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | à l'arrivée |


## <a name="configuring-data-sources"></a>Configuration des sources de données
Vous configurez des sources de données à partir du menu **Données** dans **Paramètres avancés** de Log Analytics.  Toutes les configurations sont remises à toutes les sources connectées dans votre espace de travail.  Nous ne pouvez actuellement exclure aucun agents de cette configuration.

![Configurer les événements Windows](./media/log-analytics-data-sources/configure-events.png)

1. Dans le portail Azure, sélectionnez **Log Analytics** > votre espace de travail > **Paramètres avancés**.
2. Sélectionnez **Données**.
3. Cliquez sur la source de données à configurer.
4. Suivez le lien vers la documentation de chaque source de données dans le tableau ci-dessus pour plus d'informations sur leur configuration.


## <a name="data-collection"></a>Collecte des données
Les configurations des sources de données sont remises en quelques minutes aux agents directement connectés à Log Analytics.  Les données spécifiées sont collectées à partir de l'agent et remises directement à Log Analytics à des intervalles spécifiques à chaque source de données.  Consultez la documentation de chaque source de données pour obtenir ces informations spécifiques.

Pour les agents System Center Operations Manager d’un groupe d'administration connecté, les configurations de sources de données sont traduites en packs d'administration et remises au groupe d'administration toutes les 5 minutes par défaut.  L’agent télécharge le pack d’administration comme tout autre, et collecte les données spécifiées. Selon la source de données, soit les données sont envoyées à un serveur d’administration qui les transfère à Log Analytics, soit l’agent les envoie à Log Analytics sans passer par le serveur d’administration. Pour plus d’informations, consultez [Détails sur la collecte de données](log-analytics-add-solutions.md#data-collection-details).  Vous pouvez consulter les détails de la connexion à Operations Manager et à Log Analytics et de la modification de la fréquence à laquelle la configuration est remise dans la rubrique [Configurer l'intégration avec System Center Operations Manager](log-analytics-om-agents.md).

Si l’agent ne peut pas se connecter à Log Analytics ou à Operations Manager, il continue à collecter des données qu’il fournira lorsqu’une connexion sera établie.  Les données peuvent être perdues si le volume de données atteint la taille maximale du cache du client ou si l’agent n’est pas en mesure d’établir une connexion dans un délai de 24 heures.

## <a name="log-analytics-records"></a>Enregistrements Log Analytics
Toutes les données collectées par Log Analytics sont stockées dans l’espace de travail en tant qu'enregistrements.  Les enregistrements collectés par différentes sources de données auront leur propre jeu de propriétés et seront identifiés par leur propriété **Type** .  Consultez la documentation de chaque source de données et solution pour plus d'informations sur chaque type d'enregistrement.

## <a name="next-steps"></a>Étapes suivantes
* Découvrez les [solutions](log-analytics-add-solutions.md) qui ajoutent des fonctionnalités à Log Analytics et collectent également des données dans l’espace de travail.
* Découvrez les [recherches de journaux](log-analytics-log-searches.md) pour analyser les données collectées à partir de sources de données et de solutions.  
* Configurez des [alertes](log-analytics-alerts.md) qui vous informent de façon proactive des données critiques collectées à partir de sources de données et solutions.
