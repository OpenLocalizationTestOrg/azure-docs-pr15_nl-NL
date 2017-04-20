
<properties
    pageTitle="Aan de aanbevelingen in batches: Machine learning aanbevelingen API | Microsoft Azure"
    description="Azure machine learning-aanbevelingen--aan de aanbevelingen in batches"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Aanbevelingen krijgen in batches

>[AZURE.NOTE] Aan de aanbevelingen in batches is ingewikkelder dan aanbevelingen één tegelijk. Controleer de API's voor informatie over hoe u aanbevelingen voor een enkele aanvraag:

> [Item naar Item aanbevelingen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Aanbevelingen van de gebruiker-naar-Item](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Batch scoren werkt alleen voor builds die na 21 juli 2016 zijn gemaakt.


Er zijn situaties waarin u nodig hebt om aanbevelingen voor meer dan één item tegelijk. Bijvoorbeeld misschien u ook bij het maken van een cache aanbevelingen of zelfs analyseren van de soorten aanbevelingen die u op het punt.

Batch score bewerkingen uitvoeren, zijn zoals verwijst naar deze asynchrone bewerkingen. U moet de aanvraag indienen, wacht totdat de bewerking is voltooid en vervolgens uw resultaten te verzamelen.  

Meer nauwkeurig, dit zijn de stappen te volgen:

1.  Een container Azure Storage maken als u nog niet hebt.
2.  Upload een invoer bestand met een beschrijving van elk van uw aanbeveling aanvragen voor Azure-blobopslag.
3.  Begin de score batchtaak.
4.  Wacht totdat de asynchrone bewerking om te voltooien.
5.  Wanneer de bewerking is voltooid, moet u de resultaten van Blob storage besturingselementen verzamelen.

Laten we begeleiden tot en met elk van deze stappen.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Een container opslag maken als u nog niet hebt

Ga naar de [Azure-portal](https://portal.azure.com) en maak een nieuw account voor de opslag als u nog niet hebt. Ga hiervoor naar **Nieuw** > **gegevens** + **opslag** > **Opslag-Account**.

Nadat u een opslag-account hebt, moet u de blob containers waar u wilt opslaan en de uitvoer van de Batchuitvoering maken.

Het bestand input.json elk van uw aanbeveling aanvraagt met Blob storage--we hier belt u Upload een invoer-bestand waarmee wordt beschreven.
Nadat u een container hebt, moet u een bestand met een beschrijving van elk van de aanvragen die u nodig hebt om uit te voeren van de service aanbevelingen uploaden.

Een batch kan slechts één type aanvraag uitvoeren van een specifieke opbouwen. Het definiëren van deze informatie in het volgende gedeelte wordt uitgelegd. Nu we is ervan uitgegaan dat we item aanbevelingen afmelden bij een specifieke opbouwen zult uitvoeren. De invoer bestand bevat vervolgens de invoergegevens (in dit geval de items zaad) voor elk van de aanvragen.

Dit is een voorbeeld van hoe het bestand input.json eruitziet:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Zoals u ziet, is het bestand een JSON-bestand, waarbij elk van de aanvragen de informatie die nodig is voor het verzenden van een aanvraag aanbevelingen heeft. Maak een soortgelijke JSON-bestand voor de vergaderverzoeken die u nodig hebt om te voldoen aan en kopieert u dit naar de container die u zojuist hebt gemaakt in blobopslag.

## <a name="kick-start-the-batch-job"></a>Begin de batchtaak

De volgende stap is een nieuwe batchtaak indienen. Voor meer informatie raadpleegt u de [API verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

De hoofdtekst van het verzoek van de API moet definiëren de locaties waar de invoer, uitvoer en foutbestanden moeten worden opgeslagen. Dit moet ook de aanmeldingsgegevens die nodig zijn voor toegang tot deze locaties definiëren. Daarnaast moet u enkele parameters opgeven die van toepassing op de hele batch (het type van aanbevelingen voor het aanvragen, het model/bouwen kunt gebruiken, het aantal resultaten per gesprek, enzovoort).

Dit is een voorbeeld van hoe het hoofdgedeelte van de aanvraag eruit moet:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Hier enkele belangrijke punten onthouden:

-   Op dit moment moeten **authenticationType** altijd worden ingesteld **PublicOrSas**.

-   U moet een token gedeeld Access handtekening (SA's) toe te staan dat de API aanbevelingen te lezen en schrijven van/naar uw Blob storage-account. Meer informatie over het genereren van tokens SA's vindt u op [de pagina aanbevelingen API](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   De enige **apiName** die momenteel wordt ondersteund is **ItemRecommend**, die wordt gebruikt voor het Item naar Item aanbevelingen. Batchen ondersteunt momenteel geen aanbevelingen van de gebruiker-naar-Item.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Wacht totdat de asynchrone bewerking om te voltooien

Wanneer u de batchbewerking begint, is het antwoord geeft als resultaat de bewerking-locatie koptekst waarmee u de gegevens die nodig zijn voor het bijhouden van de bewerking is.
U bijhouden de bewerking met behulp van de [API voor bewerking Status ophalen]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da), net zoals u voor het bijhouden van de werking van een opbouwen-bewerking uitvoeren.

## <a name="get-the-results"></a>De resultaten krijgen

Nadat de bewerking is voltooid, ervan uitgaande dat er geen fouten waren, kunt u de resultaten van uw uitvoer verzamelen Blob storage.

In het onderstaande voorbeeld weergeven wat de uitvoer als volgt uitzien. In dit voorbeeld ziet resultaten voor een batch met slechts twee aanvragen (voor kort te houden).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Meer informatie over de beperkingen

-   Slechts één batchtaak kan worden aangeroepen per abonnement tegelijk.
-   Een batch taak invoer-bestand kan niet meer dan 2 MB.
