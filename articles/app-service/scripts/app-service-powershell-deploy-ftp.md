---
title: Exemple de script Azure PowerShell - Télécharger des fichiers vers une application web via FTP | Microsoft Docs
description: Exemple de script Azure PowerShell - Télécharger des fichiers vers une application web via FTP
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: b7d46d6f-44fd-454c-8008-87dab6eefbc1
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 87a8a86fc0d655ae3881613ef89a31b8cf85219d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31586872"
---
# <a name="upload-files-to-a-web-app-using-ftp"></a>Télécharger des fichiers vers une application web via FTP

Cet exemple de script crée une application web dans App Service avec ses ressources associées, puis déploie votre code d’application web au moyen du site FTP (via [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).

Si nécessaire, installez Azure PowerShell à l’aide des instructions figurant dans le [Guide Azure PowerShell](/powershell/azure/overview), puis exécutez `Connect-AzureRmAccount` pour créer une connexion avec Azure.

## <a name="sample-script"></a>Exemple de script

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "Upload files to a web app using FTP")]

## <a name="clean-up-deployment"></a>Nettoyer le déploiement 

Une fois l’exemple de script exécuté, la commande suivante permet de supprimer le groupe de ressources, l’application web et toutes les ressources associées.

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Crée un plan App Service. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Crée une application web. |
| [Get-AzureRmWebAppPublishingProfile](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | Obtient un profil de publication d’application web. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le module Azure PowerShell, consultez la [Documentation Azure PowerShell](/powershell/azure/overview).

Vous trouverez des exemples supplémentaires de scripts Azure PowerShell pour Azure App Service Web Apps sur la page [Azure PowerShell Samples](../app-service-powershell-samples.md) (Exemples Azure PowerShell).
