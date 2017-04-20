<properties 
    pageTitle="Hoe Blitline gebruikt voor afbeelding verwerking - Azure handleiding aanbevelen" 
    description="Informatie over het gebruik van de service Blitline verwerken van afbeeldingen in een Azure-toepassing." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Het gebruik van Blitline met Azure en Azure Storage

Deze handleiding wordt uitgelegd hoe u toegang tot Blitline services en hoe taken kunnen verzenden naar Blitline.

## <a name="what-is-blitline"></a>Wat is Blitline?

Blitline is een cloudgebaseerde verwerking van afbeeldingen service waarmee met een enterprise niveau afbeelding verwerking op een deel van de prijs die u zelf maakt de kosten.

Het feit is dat verwerking van afbeeldingen is nog steeds opnieuw, meestal opnieuw gemaakt voor elke website. Wij begrijpen dit omdat we hebt gemaakt ze een miljoen tijden te. Één dag die we besloten dat misschien doen alleen we dit voor iedereen. We weten hoe u deze, moet u dit doen snel en efficiënt en iedereen werk in de tussentijd opslaan.

Zie [http://www.blitline.com](http://www.blitline.com)voor meer informatie.

## <a name="what-blitline-is-not"></a>Wat Blitline is niet...

Als u wilt verduidelijken wat Blitline is handig voor, is het meestal gemakkelijker te bepalen wat Blitline niet doet voordat u verdergaat.

- Blitline heeft geen HTML-widgets om afbeeldingen te uploaden. Openbaar of met beperkte machtigingen die beschikbaar zijn voor Blitline bereiken, moet u afbeeldingen beschikbaar hebben.

- Blitline geen live-beeld verwerkt, zoals Aviary.com

- Blitline accepteert geen uploaden van afbeeldingen, niet kunt u uw afbeeldingen rechtstreeks naar Blitline push. U moet deze push Azure Storage of andere locaties Blitline ondersteunt en vervolgens Blitline vertellen waar kunt u deze.

- Blitline massively parallel en eventuele synchrone verwerking niet doet. Wat betekent dat u moet Stuur ons een postback_url en we kunt u zien wanneer we klaar bent met verwerking.

## <a name="create-a-blitline-account"></a>Een Blitline-account maken

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Het maken van een taak Blitline

Blitline gebruikt JSON om te bepalen de acties die u wilt maken op een afbeelding. Deze JSON bestaat uit een paar eenvoudige velden.

De eenvoudigste voorbeeld is er als volgt uit:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Hier zijn er JSON die wordt pas van de afbeelding van een "src" "... boys.jpeg" en pas de grootte van die afbeelding tot en met 240 x 140.

De toepassings-ID is iets die u in uw tabblad **VERBINDINGSGEGEVENS** of **beheren** op Azure vinden kunt. Dit is uw geheime id waarmee u taken uitvoeren op Blitline.

De parameter "opslaan" identificeert informatie over waar u de afbeelding plaatsen wanneer we hebben deze verwerkt. In dit geval alledaagse nog niet hebt we gedefinieerd. Als er geen locatie is gedefinieerd Blitline wordt opgeslagen deze lokaal (en tijdelijk) op een unieke cloudlocatie bevindt. Is mogelijk aan die locatie van de JSON geretourneerd door Blitline wanneer u de Blitline. De id "afbeelding" is vereist en u wordt geretourneerd als deze name identificeren afbeelding opgeslagen.

U vindt meer informatie over de *functies* die we hier ondersteuning: <http://www.blitline.com/docs/functions>

U kunt ook de documentatie over de Taakopties hier vinden: <http://www.blitline.com/docs/api>

Nadat u uw JSON hebt u hoeft te **posten** is erop om deze`http://api.blitline.com/job`

Hier krijgt u JSON weer die er ongeveer zo uitziet:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Hier leest u dat Blitline uw aanvraag heeft ontvangen, deze heeft ingevoerd in een verwerkingswachtrij en voltooiing van de afbeelding is beschikbaar op: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Het opslaan van een afbeelding aan uw account Azure Storage

Als u een opslag van Azure-account hebt, kunt u eenvoudig Blitline push de verwerkte afbeeldingen in uw Azure container hebt. Door toe te voegen een "azure_destination" definieert u de locatie en de machtigingen voor Blitline naar te zenden.

Hier volgt een voorbeeld:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Door in te vullen de waarden CAPITALIZED met uw eigen, kunt u deze JSON naar http://api.blitline.com/job verzenden en de afbeelding 'src' wordt verwerkt met een vervagingsfilter en klik vervolgens op Azure bestemming wordt gedrukt.

###<a name="please-note"></a>Let op:

De SA's moet de volledige url in SA's, inclusief de bestandsnaam van het doelbestand bevatten.

Voorbeeld:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


U kunt ook de meest recente versie van Blitline van Azure Storage documenten lezen [hier](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Volgende stappen

Ga naar blitline.com voor meer informatie over onze andere functies:

* Blitline API eindpunt documenten <http://www.blitline.com/docs/api>
* Blitline API functies <http://www.blitline.com/docs/functions>
* Blitline API voorbeelden <http://www.blitline.com/docs/examples>
* Derde deel Nuget bibliotheek <http://nuget.org/packages/Blitline.Net>
