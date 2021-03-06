---
title: Démarrer et arrêter Azure Stack | Microsoft Docs
description: Découvrez comment démarrer et arrêter Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 53015ba5c282bbe9c7b8185b080ffb6d834b6c75
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31391131"
---
# <a name="start-and-stop-azure-stack"></a>Démarrer et arrêter Azure Stack
Suivez les procédures décrites dans cet article pour arrêter et redémarrer correctement les services Azure Stack. 

## <a name="stop-azure-stack"></a>Arrêter Azure Stack 

Pour arrêter Azure Stack, procédez comme suit :

1. Ouvrez une session PEP (Privileged Endpoint Session) à partir d’une machine ayant accès aux machines virtuelles ERCS Azure Stack via le réseau. Pour obtenir des instructions, voir [Utilisation du point de terminaison privilégié dans Azure Stack](azure-stack-privileged-endpoint.md).

2. À partir de la session PEP, exécutez :

    ```powershell
      Stop-AzureStack
    ```

3. Attendez que tous les nœuds physiques Azure Stack soient hors tension.

> [!Note]  
> Vous pouvez vérifier l’état de l’alimentation d’un nœud physique en suivant les instructions du fabricant OEM qui a fourni le matériel Azure Stack. 

## <a name="start-azure-stack"></a>Démarrer Azure Stack 

Pour démarrer Azure Stack, procédez comme suit. Suivez ces étapes quelle que soit la manière dont Azure Stack s’est arrêté.

1. Mettez tous les nœuds physiques de votre environnement Azure Stack sous tension. Découvrez comment mettre sous tension les nœuds physiques en consultant les instructions du fabricant OEM qui vous a fourni le matériel Azure Stack.

2. Attendez que les services d’infrastructure Azure Stack démarrent. Le démarrage des services d’infrastructure Azure Stack peut prendre jusqu’à deux heures. Vous pouvez vérifier l’état de démarrage d’Azure Stack à l’aide de la cmdlet [**Get-ActionStatus**](#get-the-startup-status-for-azure-stack).


## <a name="get-the-startup-status-for-azure-stack"></a>Obtenir l’état de démarrage d’Azure Stack

Pour initier la routine de démarrage Azure Stack, procédez comme suit :

1. Ouvrez une session PEP (Privileged Endpoint Session) à partir d’une machine ayant accès aux machines virtuelles ERCS Azure Stack via le réseau.

2. À partir de la session PEP, exécutez :

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Résoudre les problèmes de démarrage et d’arrêt d’Azure Stack

Si les services d’infrastructure et de locataire ne démarrent pas dans les 2 heures qui suivent la mise sous tension de votre environnement Azure Stack, procédez comme suit. 

1. Ouvrez une session PEP (Privileged Endpoint Session) à partir d’une machine ayant accès aux machines virtuelles ERCS Azure Stack via le réseau.

2. Exécutez : 

    ```powershell
      Test-AzureStack
      ```

3. Examinez la sortie et corrigez les erreurs liées à l’intégrité. Pour plus d’informations, consultez [Exécuter un test de validation d’Azure Stack](azure-stack-diagnostic-test.md).

4. Exécutez :

    ```powershell
      Start-AzureStack
    ```

5. Si l’exécution de **Start-AzureStack** entraîne un échec, contactez les services du support technique Microsoft. 

## <a name="next-steps"></a>Étapes suivantes 

Pour plus d’informations sur les outils de diagnostic Azure Stack et sur la journalisation des problèmes, consultez l’article [Outils de diagnostic Azure Stack](azure-stack-diagnostics.md).
