---
title: Gérer l’accès aux ressources Azure avec Privileged Identity Management (PIM)
description: En savoir plus sur l’utilisation du contrôle d’accès en fonction du rôle dans PIM pour accéder aux ressources Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: fb0a1ff3821efd7114b509b72e143d5240b61b4c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204207"
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Gérer l’accès aux ressources Azure avec Privileged Identity Management

Pour protéger des comptes privilégiés contre les cyberattaques malveillantes, vous pouvez utiliser Azure Active Directory Privileged Identity Management (PIM) afin de réduire le temps d’exposition des privilèges et d’augmenter la visibilité quant à leur utilisation via des rapports et des alertes. Pour cela, PIM n’autorise les utilisateurs à utiliser ces privilèges que pendant une période précise (« just in time » (JIT)), ou en leur attribuant des privilèges pour une plus courte durée après laquelle les privilèges sont automatiquement révoqués. 

Vous pouvez maintenant utiliser PIM avec le contrôle d’accès en fonction du rôle Azure (RBAC) pour gérer, contrôler et surveiller l’accès aux ressources Azure. PIM peut gérer l’appartenance des rôles intégrés et personnalisés pour vous aider à : 

- Activer à la demande l’accès aux ressources Windows Azure pendant une période précise (JIT)
- Faire automatiquement expirer l’accès aux ressources pour des utilisateurs et groupes attribués 
- Attribuer un accès temporaire à des ressources Azure pour les tâches rapides ou les planifications de l’appel
- Obtenir des alertes lorsque de nouveaux utilisateurs ou groupes se voient assigner un accès à des ressources, et lorsqu’ils activent des attributions éligibles

Pour plus d’informations, voir [Présentation du contrôle d’accès en fonction du rôle dans Azure PIM](../active-directory/privileged-identity-management/azure-pim-resource-rbac.md).