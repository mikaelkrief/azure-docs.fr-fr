---
title: Fichier Include
description: Fichier Include
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 500e335d0b2eddc56cdfb9828236bc4676d9b6aa
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371165"
---
> [!div class="op_single_selector"]
> * [C sur Windows](../articles/iot-accelerators/iot-accelerators-connecting-devices.md)
> * [C sur Linux](../articles/iot-accelerators/iot-accelerators-connecting-devices-linux.md)
> * [Node.js (générique)](../articles/iot-accelerators/iot-accelerators-connecting-devices-node.md)
> * [Node.js sur Raspberry Pi](../articles/iot-accelerators/iot-accelerators-connecting-pi-node.md)
> * [C sur Raspberry Pi](../articles/iot-accelerators/iot-accelerators-connecting-pi-c.md)

Dans ce didacticiel, vous allez implémenter un appareil **Condenseur** qui envoie les données de télémétrie ci-après à [l’accélérateur de solution](../articles/iot-accelerators/iot-accelerators-what-are-solution-accelerators.md) de surveillance à distance :

* Température
* Pression
* Humidité

Par souci de simplicité, le code génère des exemples de valeurs de données de télémétrie pour le **Condenseur**. Vous pouviez étendre l’exemple en connectant des capteurs réels sur votre périphérique et envoyer les données de télémétrie réelles.

Le périphérique en exemple également :

* Envoie des métadonnées à la solution pour décrire ses fonctionnalités.
* Répond aux actions déclenchées à partir de la page **Périphériques** dans la solution.
* Répond aux modifications de configuration envoyées à partir de la page **Périphériques** dans la solution.

Pour effectuer ce didacticiel, vous avez besoin d’un compte Azure actif. Si vous ne possédez pas de compte, vous pouvez créer un compte d’évaluation gratuit en quelques minutes. Pour plus d'informations, consultez la page [Version d'évaluation gratuite d'Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Avant de commencer

Avant d’écrire du code pour votre appareil, déployez votre accélérateur de solution de surveillance à distance, puis ajoutez un nouvel appareil physique à cette solution.

### <a name="deploy-your-remote-monitoring-solution-accelerator"></a>Déployer votre accélérateur de solution de surveillance à distance

L’appareil **Condenseur** que vous créez dans le cadre de ce didacticiel envoie des données à une instance de l’accélérateur de solution de [surveillance à distance](../articles/iot-suite/iot-suite-remote-monitoring-explore.md). Si vous n’avez pas encore approvisionné l’accélérateur de solution de surveillance à distance dans votre compte Azure, consultez l’article [Déployer l’accélérateur de solution de surveillance à distance](../articles/iot-accelerators/iot-accelerators-remote-monitoring-deploy.md).

Au terme du processus de déploiement de la solution de surveillance à distance, cliquez sur **Lancer** pour ouvrir le tableau de bord de la solution dans votre navigateur.

![Tableau de bord de la solution](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Ajouter votre appareil à la solution de surveillance à distance

> [!NOTE]
> Si vous avez déjà ajouté un appareil dans votre solution, vous pouvez ignorer cette étape. Toutefois, l’étape suivante requiert la chaîne de connexion de votre appareil. Vous pouvez récupérer la chaîne de connexion d’un appareil à partir du [portail Azure](https://portal.azure.com) ou à l’aide de l’outil d’interface de ligne de commande (CLI) [az iot](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest).

Pour qu’un appareil puisse se connecter à l’accélérateur de solution, il doit s’identifier auprès d’IoT Hub à l’aide d’informations d’identification valides. Vous avez la possibilité d’enregistrer la chaîne de connexion de l’appareil qui contient ces informations d’identification lorsque vous ajoutez l’appareil à la solution. La chaîne de connexion de l’appareil sera ajoutée dans votre application cliente dans la suite de ce didacticiel.

Pour ajouter un périphérique à votre solution de surveillance à distance, procédez comme suit dans la page **Périphériques** de la solution :

1. Choisissez **+ Nouvel appareil**, puis **Physique** comme **Type de périphérique** :

    ![Ajouter un appareil physique](media/iot-suite-selector-connecting/devicesprovision.png)

1. Entrez **Physique-condenseur** comme ID de périphérique. Choisissez les options **Clé symétrique** et **Générer automatiquement des clés** :

    ![Choisissez les options de périphérique](media/iot-suite-selector-connecting/devicesoptions.png)

1. Choisissez **Appliquer**. Notez quelque part les valeurs de l’**ID de l’appareil**, de la **Clé primaire** et de la **Clé primaire de la chaîne de connexion** :

    ![Récupérer les informations d’identification](media/iot-suite-selector-connecting/credentials.png)

Vous venez d’ajouter un appareil physique à l’accélérateur de solution de surveillance à distance et de noter sa chaîne de connexion d’appareil. Dans les sections suivantes, vous allez implémenter l’application cliente qui utilise la chaîne de connexion de l’appareil pour se connecter à votre solution.

L’application cliente met en œuvre le modèle de périphérique **Condenseur** intégré. Un modèle d’appareil d’accélérateur de solution spécifie les éléments ci-après concernant un appareil :

* Les propriétés que le périphérique signale à la solution. Par exemple, un périphérique **Condenseur** fournit des informations sur son microprogramme et son emplacement.
* Les types de données de télémétrie envoyés par le périphérique à la solution. Par exemple, un périphérique **Condenseur** envoie des valeurs sur la température, l’humidité et la pression.
* Les méthodes que vous pouvez planifier à partir de la solution pour s’exécuter sur le périphérique. Par exemple, un périphérique **Condenseur** doit mettre en œuvre des méthodes **Redémarrer**, **FirmwareUpdate**, **EmergencyValveRelease** et  **IncreasePressure**.