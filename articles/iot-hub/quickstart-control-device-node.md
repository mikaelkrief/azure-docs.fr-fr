---
title: Démarrage rapide (Node.js) pour contrôler un appareil à partir d’Azure IoT Hub | Microsoft Docs
description: Dans ce guide de démarrage rapide, vous exécutez deux exemples d’applications Node.js. Une application est une application back-end qui peut contrôler à distance des appareils connectés à votre concentrateur. L’autre application simule un appareil connecté à votre concentrateur qui peut être contrôlé à distance.
services: iot-hub
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: node
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: a3f60409d6b337ef314963d2116e71f156d6dafd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-nodejs"></a>Démarrage rapide : contrôler un appareil connecté à un IoT Hub (Node.js)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub est un service Azure qui vous permet de traiter de gros volumes de données de télémétrie envoyées par vos appareils IoT dans le cloud et de gérer vos appareils à partir du cloud. Dans ce guide de démarrage rapide, vous utilisez une *méthode directe* pour contrôler un appareil simulé connecté à votre IoT Hub. Vous pouvez utiliser les méthodes directes pour modifier à distance le comportement d’un appareil connecté à votre IoT Hub.

Ce démarrage rapide utilise deux applications Node.js prédéfinies :

* Une application d’appareil simulé qui répond aux méthodes directes appelées à partir d’une application back-end. Pour recevoir les appels de méthode directe, cette application se connecte à un point de terminaison spécifique à l’appareil sur votre IoT Hub.
* Une application back-end qui appelle les méthodes directes sur l’appareil simulé. Pour appeler une méthode directe sur un appareil, cette application se connecte à un point de terminaison côté service sur votre IoT Hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="prerequisites"></a>Prérequis


Les deux exemples d’applications que vous exécutez dans ce guide de démarrage rapide sont écrits à l’aide de Node.js. Votre machine de développement doit disposer de Node.js v4.x.x ou version ultérieure.

Vous pouvez télécharger Node.js pour plusieurs plateformes sur [nodejs.org](https://nodejs.org).

Vous pouvez vérifier la version actuelle de Node.js sur votre machine de développement à l’aide de la commande suivante :

```cmd/sh
node --version
```

Si ce n’est déjà fait, téléchargez l’exemple de projet Node.js à partir de https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip et extrayez l’archive ZIP.

## <a name="create-an-iot-hub"></a>Créer un hub IoT

Si vous avez terminé le [Démarrage rapide : envoyer des données de télémétrie à partir d’un appareil à un IoT Hub](quickstart-send-telemetry-node.md) précédent, vous pouvez ignorer cette étape.

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Inscrire un appareil

Si vous avez terminé le [Démarrage rapide : envoyer des données de télémétrie à partir d’un appareil à un IoT Hub](quickstart-send-telemetry-node.md) précédent, vous pouvez ignorer cette étape.

Un appareil doit être inscrit dans votre hub IoT pour pouvoir se connecter. Dans ce démarrage rapide, vous utilisez Azure CLI pour inscrire un appareil simulé.

1. Ajoutez l’extension CLI d’IoT Hub et créez l’identité de l’appareil. Remplacez `{YourIoTHubName}` par le nom de votre IoT Hub :

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName}--device-id MyNodeDevice
    ```

1. Exécutez la commande suivante pour obtenir la _chaîne de connexion_ à l’appareil que vous venez d’inscrire :

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyNodeDevice --output table
    ```

    Notez la chaîne de connexion à l’appareil, du type `Hostname=...=`. Vous utiliserez cette valeur plus loin dans ce démarrage rapide.

1. Vous avez également besoin d’une _chaîne de connexion de service_ pour activer l’application back-end afin de vous connecter à votre IoT Hub et récupérer les messages. La commande suivante récupère la chaîne de connexion de service correspondant à votre hub IoT :

    ```azurecli-interactive
    az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
    ```

    Notez la chaîne de connexion de service, du type `Hostname=...=`. Vous utiliserez cette valeur plus loin dans ce démarrage rapide. La chaîne de connexion de service est différente de la chaîne de connexion d’appareil.

## <a name="listen-for-direct-method-calls"></a>Écouter les appels de méthode directe

L’application d’appareil simulé se connecte à un point de terminaison spécifique de l’appareil sur votre IoT Hub, envoie les données de télémétrie simulée et écoute les appels de méthode directe provenant de votre concentrateur. Dans ce démarrage rapide, l’appel de méthode directe à partir du concentrateur indique à l’appareil de modifier la fréquence à laquelle il envoie des données de télémétrie. L’appareil simulé renvoie un accusé de réception à votre concentrateur après l’exécution de la méthode directe.

1. Dans une fenêtre de terminal, accédez au dossier racine de l’exemple de projet Node.js. Ensuite, accédez au dossier **Quickstarts\simulated-device-2**.

1. Utilisez un éditeur de texte pour ouvrir le fichier **SimulatedDevice.js**.

    Remplacez la valeur de la variable `connectionString` par la chaîne de connexion d’appareil que vous avez notée précédemment. Puis, enregistrez les modifications apportées au fichier **SimulatedDevice.js**.

1. Dans la fenêtre de terminal, exécutez les commandes suivantes pour installer les bibliothèques requises et exécuter l’application d’appareil simulé :

    ```cmd/sh
    npm install
    node SimulatedDevice.js
    ```

    La capture d’écran suivante présente la sortie lorsque l’application d’appareil simulé envoie des données de télémétrie à votre IoT Hub :

    ![Exécuter l’appareil simulé](media/quickstart-control-device-node/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Appeler la méthode directe

L’application back-end se connecte au point de terminaison côté service sur votre IoT Hub. L’application effectue des appels de méthode directe à un appareil via votre IoT Hub et écoute les accusés de réception. Une application back-end de l’IoT Hub s’exécute généralement dans le cloud.

1. Dans une autre fenêtre de terminal, accédez au dossier racine de l’exemple de projet Node.js. Puis, accédez au dossier **Quickstarts\back-end-application**.

1. Utilisez un éditeur de texte pour ouvrir le fichier **BackEndApplication.js**.

    Remplacez la valeur de la variable `connectionString` par la chaîne de connexion de service que vous avez notée précédemment. Puis, enregistrez vos modifications apportées au fichier **BackEndApplication.js**.

1. Dans la fenêtre de terminal, exécutez les commandes suivantes pour installer les bibliothèques requises et exécuter l’application back-end :

    ```cmd/sh
    npm install
    node BackEndApplication.js
    ```

    La capture d’écran suivante montre la sortie lorsque l’application effectue un appel de méthode directe sur l’appareil et reçoit un accusé de réception :

    ![Exécuter l’application back-end](media/quickstart-control-device-node/BackEndApplication.png)

    Après avoir exécuté l’application back-end, un message s’affiche dans la fenêtre de la console exécutant l’appareil simulé, et la fréquence à laquelle il envoie les messages change :

    ![Changement dans le client simulé](media/quickstart-control-device-node/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Supprimer des ressources

Si vous envisagez de consulter les tutoriels suivants, conservez le groupe de ressources et l’IoT Hub afin de les réutiliser ultérieurement.

Si vous n’avez plus besoin du hub IoT, supprimez-le ainsi que le groupe de ressources dans le portail. Pour ce faire, sélectionnez le groupe de ressources qui contient votre hub IoT, puis cliquez sur **Supprimer**.

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez appelé une méthode directe sur un appareil à partir d’une application back-end et répondu à l’appel de méthode directe dans une application d’appareil simulé.

Pour savoir comment acheminer les messages appareil-à-cloud vers différentes destinations dans le cloud, passez au tutoriel suivant.

> [!div class="nextstepaction"]
> [Tutoriel : acheminer les données de télémétrie vers différents points de terminaison pour traitement](iot-hub-node-node-process-d2c.md)