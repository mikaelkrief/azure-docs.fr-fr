---
title: Sécuriser votre déploiement de l’Internet des objets | Microsoft Docs
description: Cet article explique comment sécuriser votre déploiement IoT
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: dobett
ms.openlocfilehash: 9422a4ce316053b6560f1e945de7d8018e52c15b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659436"
---
[!INCLUDE [iot-secure-your-deployment](../../includes/iot-secure-your-deployment.md)]

## <a name="iot-solution-accelerator-cipher-suites"></a>Suites de chiffrement de l’accélérateur de solution IoT

Les accélérateurs de solution IoT prennent en charge les suites de chiffrement ci-dessous (dans cet ordre).

| Suite de chiffrement | Longueur |
| --- | --- |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. 7680 bits RSA) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. 3072 bits RSA) FS |128 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. 7680 bits RSA) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. 3072 bits RSA) FS |128 |
| TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="see-also"></a>Voir aussi
Vous pouvez également explorer certaines des autres fonctionnalités des accélérateurs de solution IoT :

* [Présentation des accélérateurs de solution de maintenance prédictive][lnk-predictive-overview]
* [Questions fréquentes (FAQ) sur les accélérateurs de solution IoT][lnk-faq]

Pour en savoir plus sur la sécurité d’IoT Hub, consultez la rubrique [Contrôler l’accès à IoT Hub][lnk-devguide-security] du guide du développeur IoT Hub.

[lnk-predictive-overview]:iot-accelerators-predictive-overview.md
[lnk-faq]:iot-accelerators-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md
