---
title: Utilisation de clés SSH avec Windows pour les machines virtuelles Linux | Microsoft Docs
description: Apprenez à créer et à utiliser des clés SSH sur un ordinateur Windows pour vous connecter à une machine virtuelle Linux dans Azure.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: danlep
ms.openlocfilehash: d0762f80267fa927681344a3e0de78b0800c8306
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2018
ms.locfileid: "31601020"
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Comment utiliser des clés SSH avec Windows sur Azure

Cet article décrit plusieurs méthodes de génération et d’utilisation de clés SSH (secure shell) sur un ordinateur Windows pour créer et se connecter à une machine virtuelle Linux dans Azure. Pour utiliser des clés SSH à partir d’un client Linux ou macOS, consultez le guide [rapide](mac-create-ssh-keys.md) ou [détaillé](create-ssh-keys-detailed.md).

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Packages Windows et clients SSH
Vous vous connectez à des machines virtuelles Linux et gérez ces machines dans Azure à l’aide d’un *client SSH*. Les ordinateurs qui exécutent Linux ou macOS ont généralement une suite de commandes SSH pour générer et gérer les clés SSH et établir les connexions SSH. 

Les ordinateurs Windows n’ont pas toujours des commandes SSH comparables installées. Les versions de Windows 10 qui contiennent le [sous-système Windows pour Linux](https://docs.microsoft.com/windows/wsl/about) vous permettent d’exécuter des utilitaires et d’y accéder, comme un client SSH intégré nativement dans un interpréteur de commandes Bash. 

Si vous voulez utiliser un autre outil que Bash pour Windows, voici une liste des packages de clients SSH Windows que vous pouvez installer localement :

* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [Git pour Windows](https://git-for-windows.github.io/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Vous pouvez aussi utiliser les utilitaires SSH disponibles dans Bash dans [Azure Cloud Shell](../../cloud-shell/overview.md). 

* Accédez à Cloud Shell dans votre navigateur web en entrant [https://shell.azure.com](https://shell.azure.com) ou dans le [portail Azure](https://portal.azure.com). 
* Accédez à Cloud Shell sous forme de terminal à partir de Visual Studio Code en installant [l’extension de compte Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>Création d’une paire de clés SSH
Cette section décrit deux options de création d’une paire de clés SSH sur Windows.

### <a name="create-ssh-keys-with-ssh-keygen"></a>Créer des clés SSH avec ssh-keygen

Si vous pouvez exécuter un interpréteur de commandes comme Bash pour Windows ou GitBash (ou Bash dans Azure Cloud Shell), créez une paire de clés SSH à l’aide de la commande `ssh-keygen`. Tapez la commande suivante et suivez les invites. Si une paire de clés SSH existe dans l’emplacement actuel, les fichiers sont remplacés. 

```bash
ssh-keygen -t rsa -b 2048
```

Pour plus d’informations, consultez les étapes [rapides](mac-create-ssh-keys.md) ou [détaillées](create-ssh-keys-detailed.md) pour créer les clés avec `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>Créer des clés SSH avec PuTTYgen

Si vous préférez utiliser un outil GUI pour créer les clés SSH, vous pouvez utiliser le générateur de clé PuTTYgen, inclus dans le [package de téléchargement de PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

Pour créer une paire de clés SSH RSA avec PuTTYgen :

1. Démarrez PuTTYgen.

2. Cliquez sur **Générer**. Par défaut, PuTTYgen génère une clé SSH-2 RSA 2048 bits.

4. Survolez la zone vide pour donner un caractère aléatoire à la clé.

5. Une fois la clé publique générée, vous pouvez entrer une phrase secrète et la confirmer. Vous êtes invité à entrer la phrase secrète quand vous vous authentifiez sur la machine virtuelle avec votre clé SSH. Sans cette phrase secrète, toute personne qui récupère votre clé privée peut se connecter à une machine virtuelle ou un service utilisant cette clé. Nous vous recommandons de créer une phrase secrète. Toutefois, si vous oubliez cette phrase secrète, il sera impossible de la récupérer.

6. La clé publique apparaît en haut de la fenêtre. Vous copiez et collez cette clé publique (sous forme d’une seule ligne) dans le portail Azure ou dans un modèle Azure Resource Manager quand vous créez une machine virtuelle Linux. Vous pouvez également cliquer sur **Enregistrer la clé publique** pour enregistrer une copie sur votre ordinateur :

    ![Enregistrer le fichier de clé publique PuTTY](./media/ssh-from-windows/save-public-key.png)

7. Pour enregistrer la clé privée au format de clé privée PuTTy (fichier .ppk), vous pouvez aussi cliquer sur **Enregistrer la clé privée**. Vous avez besoin du fichier .ppk si vous voulez utiliser PuTTY par la suite pour établir une connexion SSH à la machine virtuelle.

    ![Enregistrer le fichier de clé privée PuTTY](./media/ssh-from-windows/save-ppk-file.png)

    Si vous voulez enregistrer la clé privée au format OpenSSH, le format de clé privée utilisé par de nombreux clients SSH, cliquez sur **Conversions** > **Exporter la clé OpenSSH**.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Fournir la clé publique SSH pendant le déploiement d’une machine virtuelle

Pour créer une machine virtuelle Linux qui utilise des clés SSH pour l’authentification, fournissez votre clé publique SSH à la création de la machine virtuelle à l’aide du portail Azure ou d’autres méthodes.

L’exemple suivant montre comment copier et coller cette clé publique dans le portail Azure lorsque vous créez une machine virtuelle Linux. Généralement, la clé publique est ensuit stockée dans `~/.ssh/authorized_keys` sur votre nouvelle machine virtuelle.

   ![Utiliser la clé publique lorsque vous créez une machine virtuelle dans le portail Azure](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Connexion à votre machine virtuelle

L’une des méthode permettant d’établir une connexion SSH à votre machine virtuelle Linux à partir de Windows est d’utiliser un client SSH. Il s’agit de la méthode par défaut si vous avez un client SSH installé sur votre système Windows ou si vous utilisez des outils SSH dans Bash dans Azure Cloud Shell. Si vous préférez un outil GUI, vous pouvez vous connecter avec PuTTY.  

### <a name="use-an-ssh-client"></a>Utiliser un client SSH
Une fois la clé publique déployée sur votre machine virtuelle Azure et la clé privée sur votre système local, établissez une connexion SSH sur votre machine virtuelle à l’aide de l’adresse IP ou du nom DNS de votre machine virtuelle. Remplacez *azureuser* et *myvm.westus.cloudapp.azure.com* dans la commande suivante par le nom d’administrateur et le nom de domaine complet (ou adresse IP) :

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Si vous avez configuré une phrase secrète quand vous avez créé votre paire de clés, entrez-la quand vous y êtes invité pendant le processus de connexion.

### <a name="connect-with-putty"></a>Se connecter avec PuTTY

Si vous avez installé le [package de téléchargement de PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) et avez généré une clé privée PuTTY (fichier .ppk), vous pouvez vous connecter à la machine virtuelle Linux avec PuTTY.

1. Démarrez PuTTY.

2. Renseignez le nom d’hôte ou l’adresse IP de votre machine virtuelle à partir du portail Azure :

    ![Ouvrir une nouvelle connexion PuTTY](./media/ssh-from-windows/putty-new-connection.png)

3. Avant de sélectionner **Ouvrir**, cliquez sur l’onglet **Connexion** > **SSH** > **Auth**. Recherchez et sélectionnez votre clé privée PuTTY (fichier .ppk) :

    ![Sélectionner votre clé privée PuTTY pour l’authentification](./media/ssh-from-windows/putty-auth-dialog.png)

4. Cliquez sur **Ouvrir** pour vous connecter à votre machine virtuelle.

## <a name="next-steps"></a>Étapes suivantes

* Pour connaître les étapes détaillées, les options et des exemples avancés d’utilisation de clés SSH, consultez [Étapes détaillées pour créer des paires de clés SSH](create-ssh-keys-detailed.md).

* Vous pouvez également utiliser PowerShell dans Azure Cloud Shell pour générer des clés SSH et établir des connexions SSH sur des machines virtuelles Linux. Consultez [Démarrage rapide de PowerShell](../../cloud-shell/quickstart-powershell.md#ssh).

* Si vous avez des difficultés à utiliser SSH pour vous connecter à vos machines virtuelles Linux, consultez [Résoudre les connexions SSH à une machine virtuelle Azure Linux](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
