---
title: Fichier Include
description: Fichier Include
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 05/03/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 2464fa41deaa1e9c43e5f53e1a900ca11b582040
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33901293"
---
**Exigences des serveur de configuration/de traitement**

**Composant** | **Prérequis** 
--- | ---
**MATÉRIEL** | 
**Cœurs de processeur** | 8 
**RAM** | 16 Go
**Nombre de disques** | 3, y compris le disque du système d’exploitation, le disque de cache du serveur de processus et le lecteur de conservation pour la restauration automatique 
**Espace disque disponible (cache du serveur de traitement)** | 600 Go
**Espace disque disponible (disque de rétention)** | 600 Go
**LOGICIEL** | 
**Système d’exploitation** | Windows Server 2012 R2 <br> Windows Server 2016
**Paramètres régionaux du système d’exploitation** | Anglais (en-us)
**Rôles Windows Server** | N’activez pas ces rôles : <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
**Stratégies de groupe** | N’activez pas ces stratégies de groupe : <br> - Empêcher l’accès à l’invite de commandes <br> - Empêcher l’accès aux outils de modification du Registre <br> - Logique de confiance pour les pièces jointes <br> - Activer l’exécution des scripts <br> [En savoir plus](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
**IIS** | - Aucun site web par défaut préexistant <br> - Aucune application/aucun site web préexistants ne doivent écouter le port 443 <br>- Activer [l’authentification anonyme](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - Activer le paramètre [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) 
**RÉSEAU** | 
**Type d’adresse IP** | Statique 
**Accès à Internet** | Le serveur doit également accéder à ces URL (directement ou par le biais d’un proxy) <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (si vous configurez un serveur de configuration) <br> - time.nist.gov <br> - time.windows.com 
**Ports** | 443 (Orchestration du canal de contrôle)<br>9443 (Transport de données) 
****Type de carte réseau** VMWARE (quand vous configurez le serveur de configuration/de traitement comme **
 machine virtuelle VMware) | VMXNET3  
**VMware vSphere PowerCLI en cours d’exécution sur la machine virtuelle* | [PowerCLI version 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")

**Exigences de dimensionnement des serveurs de configuration/de traitement**

**UC** | **Mémoire** | **Disque cache** | **Taux de modification des données** | **Machines répliquées**
--- | --- | --- | --- | ---
8 processeurs virtuels<br/><br/> 2 sockets * 4 cœurs à 2,5 GHz | 16 Go | 300 Go | 500 Go ou moins | < 100 machines
12 processeurs virtuels<br/><br/> 2 socks * 6 cœurs à 2,5 GHz | 18 Go | 600 Go | 500 Go à 1 To | 100 à 150 machines
16 processeurs virtuels<br/><br/> 2 socks * 8 cœurs à 2,5 GHz | 32 Go | 1 To | 1 à 2 To | 150 à 200 machines

