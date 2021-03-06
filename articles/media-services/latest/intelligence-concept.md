---
title: Intelligence multimédia Azure | Microsoft Docs
description: Lorsque vous utilisez Azure Media Services, vous pouvez analyser vos contenus audio et vidéo à l’aide d’AudioAnalyzerPreset et de VideoAnalyzerPreset.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/24/2018
ms.author: juliako
ms.openlocfilehash: 804a418f6ee88974d6e74a2c18bc5d01b6adf838
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33783548"
---
# <a name="media-intelligence"></a>Intelligence multimédia

L’API REST Azure Media Services v3 vous permet d’analyser vos contenus audio et vidéo. Pour analyser votre contenu, vous devez créer une **transformation** et envoyer un **travail** qui utilise l’un de ces paramètres prédéfinis : **AudioAnalyzerPreset** ou **VideoAnalyzerPreset** . 

## <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

**AudioAnalyzerPreset** vous permet d’extraire plusieurs insights audio d’un fichier audio ou vidéo. La sortie inclut un fichier JSON (avec tous les insights) et un fichier VTT pour la transcription audio. Ce paramètre accepte une propriété qui spécifie la langue du fichier d’entrée sous la forme d’une chaîne [BCP47](https://tools.ietf.org/html/bcp47). Les analyses audio sont les suivantes :

* Transcription audio : transcription des mots prononcés avec horodatages. Plusieurs langues sont prises en charge
* Indexation de l’orateur : mappage des orateurs et des mots prononcés correspondants
* Analyse du sentiment vocal : sortie de l’analyse des sentiments effectuée sur la transcription audio
* Mots clés : mots clés extraits de la transcription audio.

## <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

**VideoAnalyzerPreset** vous permet d’extraire plusieurs insights audio et vidéo à partir d’une vidéo. La sortie inclut un fichier JSON (avec tous les insights), un fichier VTT pour la transcription audio et une collection de miniatures. Ce paramètre accepte également une chaîne [BCP47](https://tools.ietf.org/html/bcp47) (représentant la langue de la vidéo) en tant que propriété. Les insights vidéo incluent tous les insights audio mentionnés ci-dessus en complément des éléments suivants :

* Suivi du visage : durée pendant laquelle des visages sont présentes dans la vidéo. Chaque visage est associé à un identifiant de visage et à une collection de miniatures correspondante
* Texte visuel : texte détecté par la reconnaissance optique des caractères. Le texte est horodaté et également utilisé pour extraire des mots clés (en plus de la transcription audio)
* Images clés : collection d’images clés extraites de la vidéo
* Modération du contenu visuel : partie des vidéos qui a été marquée d’un drapeau l’identifiant comme un contenu pour adulte ou provocateur par nature
* Annotation : résultat de l’annotation des vidéos sur la base d’un modèle d’objet prédéfini

##  <a name="insightsjson-elements"></a>Éléments insights.json

La sortie inclut un fichier JSON (insights.json) contenant tous les insights trouvés dans le contenu vidéo ou audio. Ce fichier json peut contenir les éléments suivants :

### <a name="transcript"></a>transcription

|NOM|Description|
|---|---|
|id|ID de la ligne.|
|text|La transcription proprement dite.|
|Langage|La langue de la transcription. Permet de prendre en charge la transcription lorsque chaque ligne peut avoir une langue différente.|
|instances|Liste des intervalles de temps pendant lesquels cette ligne est apparue. Si l’instance est une transcription, il n’y aura qu’1 seule instance.|

Exemple :

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

### <a name="ocr"></a>ocr

|NOM|Description|
|---|---|
|id|ID de la ligne OCR.|
|text|Texte de l’OCR.|
|confidence|Degré de confiance de la reconnaissance.|
|Langage|Langue de l’OCR.|
|instances|Liste des intervalles de temps au cours desquels cette OCR est apparue (la même OCR peut apparaître plusieurs fois).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

### <a name="keywords"></a>mots clés

|NOM|Description|
|---|---|
|id|ID du mot clé.|
|text|Texte du mot clé.|
|confidence|Degré de confiance de la reconnaissance du mot clé.|
|Langage|Langue du mot clé (si traduction).|
|instances|Liste des intervalles de temps pendant lesquels ce mot clé est apparu (un mot clé peut apparaître plusieurs fois).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

### <a name="faces"></a>visages

|NOM|Description|
|---|---|
|id|ID du visage.|
|Nom|Nom du visage. Il peut avoir la valeur 'Unknown #0' ou il peut s’agit d’une célébrité identifiée ou une personne formée par le client.|
|confidence|Degré de confiance de l’identification du visage.|
|description|Dans le cas d’une célébrité, sa description (« Satya Nadella est né à...»). |
|thumbnalId|ID de l’image miniature de ce visage.|
|knownPersonId|Dans le cas d’une personne connue, son ID interne.|
|referenceId|Dans le cas d’une célébrité Bing, son ID Bing.|
|referenceType|Bing uniquement (pour le moment).|
|title|Dans le cas d’une célébrité, son titre (par exemple « PDG de Microsoft »).|
|imageUrl|Dans le cas d’une célébrité, l’URL de l’image associée.|
|instances|Instances où la visage est apparu dans l’intervalle de temps donné. Chaque instance possède également un thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

### <a name="labels"></a>étiquettes

|NOM|Description|
|---|---|
|id|ID d’étiquette.|
|Nom|Nom de l’étiquette (par exemple, « ordinateur », « TV »).|
|Langage|Langue du nom de l’étiquette (si traduction). BCP-47|
|instances|Liste des intervalles de temps au cours desquels cette étiquette est apparue (une étiquette peut apparaître plusieurs fois). Chaque instance possède un champ de confiance. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

### <a name="shots"></a>captures

|NOM|Description|
|---|---|
|id|ID de la capture.|
|keyFrames|Liste des images clés au sein de la capture (chacune possède un ID et une liste d’intervalles de temps d’instances).|
|instances|Liste des intervalles de temps de cette capture (les captures n’ont qu’1 seule instance).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

### <a name="audioeffects"></a>audioEffects

|NOM|Description|
|---|---|
|id|ID de l’effet audio.|
|Type|Type d’effet audio (par exemple, applaudissements, discours, silence).|
|instances|Liste des intervalles de temps au cours desquels cet effet audio est apparu.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```


### <a name="sentiments"></a>sentiments

Les sentiments sont regroupés par leur champ sentimentType (neutre/positif/négatif). Par exemple, 0-0.1, 0.1-0.2.

|NOM|Description|
|---|---|
|id|ID du sentiment.|
|averageScore |Moyenne de tous les résultats obtenus pour toutes les instances de ce type de sentiment : neutre/positif/négatif|
|instances|Liste des intervalles de temps au cours desquels ce sentiment est apparu.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Didacticiel : analyser des vidéos avec Azure Media Services](analyze-videos-tutorial-with-api.md)
