---
title: Compétence de recherche cognitive OCR (Recherche Azure) | Microsoft Docs
description: Extrayez le texte de fichiers image dans un pipeline d’enrichissement Recherche Azure.
services: search
manager: pablocas
author: luiscabrer
documentationcenter: ''
ms.assetid: ''
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 4ac86be25ebe7d12190cce290603ce83037a7b47
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640368"
---
# <a name="ocr-cognitive-skill"></a>Compétence cognitive OCR

La compétence **OCR** extrait le texte de fichiers d’image. Les formats de fichiers pris en charge sont les suivants :

+ .JPEG
+ .JPG
+ .PNG
+ .BMP
+ .GIF


## <a name="skill-parameters"></a>Paramètres de la compétence

Les paramètres respectent la casse.

| Nom du paramètre     | Description |
|--------------------|-------------|
| detectOrientation | Active la détection automatique de l’orientation de l’image. <br/> Valeurs valides : true, false.|
|defaultLanguageCode |  Code de langue du texte d’entrée. Les langues prises en charge sont les suivantes : `ar, cs, da, de, en, es, fi, fr, he, hu, it, ko, pt-br, pt`.  Si le code de langue n’est pas spécifié ou est Null, la langue est détectée automatiquement.|
| textExtractionAlgorithm | « printed » (imprimé) ou « handwritten » (manuscrit). L’algorithme OCR de reconnaissance de texte « handwritten » est actuellement en préversion et n’est pris en charge qu’en anglais. |

## <a name="skill-inputs"></a>Entrées de la compétence

| Nom d’entrée      | Description                                          |
|---------------|------------------------------------------------------|
| image         | Type complexe. Ne fonctionne qu’avec le champ « /documents/normalized_images », généré par l’indexeur d’objets Blob Azure quand ```imageAction``` a la valeur ```generateNormalizedImages```. Pour plus d’informations, consultez [l’exemple](#sample-output).|


## <a name="skill-outputs"></a>Sorties de la compétence
| Nom de sortie     | Description                   |
|---------------|-------------------------------|
| text          | Texte brut extrait de l’image.   |
| layoutText    | Type complexe qui décrit le texte extrait, ainsi que l’emplacement où le texte a été trouvé.|


## <a name="sample-definition"></a>Exemple de définition

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Exemple de sortie text et layoutText

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Exemple : fusion du texte extrait d’images incorporées avec le contenu du document.

La fusion de texte permet notamment de fusionner la représentation textuelle d’images (texte issu d’une compétence OCR ou légende d’une image) dans le champ de contenu d’un document. 

L’exemple d’ensemble de compétences suivant crée un champ *merged_text* destiné à accueillir le contenu textuel de votre document, ainsi que le texte extrait par traitement OCR de chacune des images incorporées dans le document. 

#### <a name="request-body-syntax"></a>Syntaxe du corps de la demande
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```
L’exemple d’ensemble de compétences ci-dessus suppose l’existence d’un champ d’images normalisées. Pour générer ce champ, définissez la configuration *imageAction* dans la définition de l’indexeur sur *generateNormalizedImages* comme indiqué ci-dessous :

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>Voir aussi
+ [Compétences prédéfinies](cognitive-search-predefined-skills.md)
+ [Compétence Fusion de texte](cognitive-search-skill-textmerger.md)
+ [Guide pratique pour définir un ensemble de compétences](cognitive-search-defining-skillset.md)
+ [Créer un indexeur (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
