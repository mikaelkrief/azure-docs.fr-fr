---
title: Démarrer et arrêter le Kit de développement Azure Stack (ASDK) | Microsoft Docs
description: Découvrez comment démarrer et arrêter le Kit de développement Azure Stack (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: dfb565803746ecdda9b36a4e12a3c3f2b4d9e0d0
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31426287"
---
# <a name="start-and-stop-the-azure-stack-development-kit-asdk"></a>Démarrer et arrêter le Kit de développement Azure Stack (ASDK)
Il n’est pas recommandé de simplement redémarrer l’ordinateur hôte ASDK. Au lieu de cela, suivez les procédures décrites dans cet article pour arrêter et redémarrer correctement les services ASDK. 

## <a name="stop-azure-stack"></a>Arrêter Azure Stack 
Pour arrêter correctement les services Azure Stack et l’ordinateur hôte ADSK, exécutez les commandes PowerShell suivantes :

1. Connectez-vous en tant qu’AzureStack\CloudAdmin sur l’ordinateur hôte du kit ASDK.
2. Ouvrez PowerShell en tant qu’administrateur (et non PowerShell ISE).
3. Exécutez les commandes suivantes afin d’établir une session de point de terminaison privilégié (PEP) : 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Ensuite, dans la session PEP, utilisez l’applet de commande **Stop-AzureStack** pour arrêter les services Azure Stack et l’ordinateur hôte ASDK :

   ```powershell
   Stop-AzureStack
   ```
5. Examinez la sortie PowerShell afin de vous assurer que les services Azure Stack sont arrêtés avant l’ordinateur hôte ASDK. Le processus d’arrêt prend quelques minutes.

## <a name="start-azure-stack"></a>Démarrer Azure Stack 
Les services ASDK doivent démarrer automatiquement après le démarrage de l’ordinateur hôte. Toutefois, le délai de démarrage des services de l’infrastructure ASDK diffère en fonction des performances de la configuration matérielle de l’ordinateur hôte ASDK. Dans certains cas, le redémarrage de l’ensemble des services peut nécessiter plusieurs heures.

Indépendamment de la manière dont les services ASDK ont été arrêtés, vous devez suivre la procédure suivante pour vous assurer que l’ensemble des services Azure Stack ont été redémarrés et sont pleinement opérationnels une fois l’ordinateur hôte mis sous tension : 

1. Mettez sous tension l’ordinateur hôte ASDK. 
2. Connectez-vous en tant qu’AzureStack\CloudAdmin sur l’ordinateur hôte du kit ASDK.
3. Ouvrez PowerShell en tant qu’administrateur (et non PowerShell ISE).
4. Exécutez les commandes suivantes afin d’établir une session de point de terminaison privilégié (PEP) :

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Ensuite, dans la session PEP, exécutez les commandes suivantes pour vérifier l’état de démarrage des services Azure Stack :

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Examinez la sortie afin de vous assurer du redémarrage approprié des services Azure Stack.

Pour en savoir plus sur les procédures recommandées d’arrêt et de redémarrage des services Azure Stack, consultez la section [Démarrer et arrêter Azure Stack](.\.\azure-stack-start-and-stop.md). 

## <a name="troubleshoot-startup-and-shutdown"></a>Résoudre les problèmes de démarrage et d’arrêt 
Exécutez ces étapes si les services Azure Stack ne démarrent pas correctement dans les 2 heures suivant la mise sous tension de votre ordinateur hôte ASDK :

1. Connectez-vous en tant qu’AzureStack\CloudAdmin sur l’ordinateur hôte du kit ASDK.
2. Ouvrez PowerShell en tant qu’administrateur (et non PowerShell ISE).
3. Exécutez les commandes suivantes afin d’établir une session de point de terminaison privilégié (PEP) :

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Ensuite, dans la session PEP, exécutez les commandes suivantes pour vérifier l’état de démarrage des services Azure Stack :

   ```powershell
   Test-AzureStack
   ```
5. Examinez la sortie et corrigez les éventuelles erreurs. Pour plus d’informations, consultez [Exécuter un test de validation d’Azure Stack](.\.\azure-stack-diagnostic-test.md).
6. Redémarrez les services Azure Stack à partir de la session PEP en exécutant l’applet de commande **Start-AzureStack** :

   ```powershell
   Start-AzureStack
   ```

Si l’exécution de l’applet **Start-AzureStack** provoque une défaillance, consultez le [forum du support Azure Stack](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack) afin de bénéficier de recommandations sur la résolution des problèmes liés à votre instance ASDK. 

## <a name="next-steps"></a>Étapes suivantes 
Pour plus d’informations sur l’outil de diagnostic Azure Stack et la journalisation des erreurs, consultez la section [Outils de diagnostic Azure Stack](.\.\azure-stack-diagnostics.md).
