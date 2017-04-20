<properties
    pageTitle="Samenstellen van een model met de UI Recommnendations | Microsoft Azure"
    description="Azure aanbevelingen van Machine Learning - bouwen van een model met de gebruikersinterface aanbevelingen"
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
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Samenstellen van een model met de gebruikersinterface aanbevelingen

Dit document is een stapsgewijze handleiding. Ons doel is u begeleid bij de stapsgewijze instructies voor het trainen van een de [Aanbevelingen UI](https://recommendations-portal.azurewebsites.net/)met model.
Via de oefening, kunt u meer informatie over het proces voor het samenstellen van een model voordat u een programma kunt doen. Deze ook familiarizes u met de gebruikersinterface, die is handig wanneer u gaat gebruiken van de service.

Deze oefening duurt ongeveer 30 minuten.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Stap 1: aanmelden in de aanbevelingen UI ##

1. Als u dit nog niet hebt gedaan, moet u [aanmelden](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) voor een nieuwe [Aanbevelingen API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) -abonnement. U kunt zich aanmeldt voor de service op **Azure** op [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) en meld u aan met uw Azure-account. Vindt u gedetailleerde instructies voor het aanmeldingsproces in *taak 1* van de [Handleiding aan de slag](cognitive-services-recommendations-quick-start.md) 

1. Wanneer u een **toets** hebt ontvangen voor uw abonnement aanbevelingen API, gaat u naar [Aanbevelingen UI](https://recommendations-portal.azurewebsites.net/). 

1. Meld u aan bij de portal door in te voeren van uw code in het veld **Accountsleutel** en klik vervolgens op de knop **aanmelden** .

    ![Aanbevelingen UI: Aanmelden dialoogvenster.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Stap 2: laten we eens enkele trainingsgegevens verzamelen ##

Voordat u een opbouwen maken kunt, moet twee soorten informatie: een catalogusbestand en een set bestanden voor gebruik. 

De catalogusbestand bevat informatie over de items die u naar de klant zijn aanbod. Een bestand gebruik bevat informatie over hoe deze items worden gebruikt, of de transacties van uw bedrijf.

Meestal wilt u uw database store voor deze soorten informatie zoeken. Tegenwoordig kunnen hebben we enkele voorbeeldgegevens verstrekt voor u zodat u kunt informatie over het gebruik van de API aanbevelingen.

U kunt de gegevens downloaden van [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopiëren en het bestand **Books.Zip** uitpakken naar een map op uw lokale computer. Bijvoorbeeld **c:\data**.

In het [verzamelen van gegevens naar uw Model trainen](cognitive-services-recommendations-collecting-data.md) -artikel vindt u gedetailleerde informatie over het schema van de bestanden die catalogus en het gebruik.
 
We gaan werken met een kleine bestanden, zodat er geen zeer te lang wachten aan training voor deze oefening. Als u een meer natuurlijke bestand proberen wilt, hebben we ook **MsStoreData.zip** met steekproef transacties in de Microsoft Store op [dezelfde locatie](http://aka.ms/RecoSampleData)geplaatst.

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Stap 3: een project maken en uploaden catalogus en het gebruik van gegevens ##

Bij het aanmelden bij de [Aanbevelingen UI](https://recommendations-portal.azurewebsites.net/), ziet u de pagina projecten. Als u projecten eerder hebt gemaakt, moet u ze hier zien.
Een project (ook wel bekend als *een model* in de verwijzing API) is een container voor uw gegevens catalogus en het gebruik. U kunt verschillende *genereert* binnen het project. We begeleiden u de stappen uit in de volgende stappen.

1. Om een nieuw project maken, typt u de naam in het tekstvak (hebben bijvoorbeeld "MyFirstModel" geschikte is) en klik op **Project toevoegen**.
 
    ![Aanbevelingen UI: Projectpagina.][reco_projects]

1. Nadat het project wordt gemaakt, klikt u op de knop **Zoeken naar een bestand** in de sectie **toevoegen een catalogus-bestand** . Upload de catalogus die u in stap 2 hebt ontvangen. Als u deze hebt opgeslagen op *c:\data*, moet u naar deze map wilt navigeren.

    ![Aanbevelingen UI: Gegevens toevoegen aan een Project.][reco_firstmodel]

1. Nadat de catalogus is geüpload, klikt u op de knop **Zoeken naar een bestand** in de sectie die **Gebruik bestanden toevoegen** . Voeg het bestand usage_large.txt.

> **Wat gebeurt er als ik een grote bestanden van gegevens over zoekgebruik heb?**
>
> Alleen gebruik bestanden kleiner is dan 200 MB kunnen worden geüpload. Dat kan het systeem bevatten omhoog naar 2 GB aan transactiegegevens, zodat u kunt meer dan één bestand uploaden, indien nodig.
> Mogelijk moet u niet dat veel gegevens naar een goede model genereren, kunt u niet alleen de grootte van de gegevens die is van belang, maar de kwaliteit van de gegevens. Het is gebruikelijk om gegevens over zoekgebruik wanneer de transacties met de meeste zich alleen op een klein aantal populaire items, en er 'iets signaal' voor andere items weer te geven.

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Stap 4: doen we enkele training! ##

Nu dat u hebt geüpload zowel de catalogus en de gegevens over zoekgebruik, gaan we de engine trainen zodat deze patronen van onze gegevens vind informatie.

1.  Klik op de knop **Nieuwe opbouwen** .

1.  Selecteer **aanbevelingen** als het type opbouwen. Melding dat we een classificeren bouwen en een FBT (vaak hebt gekocht bij elkaar) ondersteunt maken als u ook typen.

    ![Aanbevelingen UI: Dialoogvenster maken.][reco_build_dialog.png]


    Een opbouwen FBT kunt u patronen voor producten die meestal gekocht/verbruikt in dezelfde transactie zijn identificeren.
    Een classificatie opbouwen wordt omgaan met rente gebruikt. 
    Er won't dieper zeer diepblauwe FBT of classificatie genereert in deze workshop, maar als u geïnteresseerd bent u moet uitchecken [bestandstypen en -model kwaliteit documentatiepagina maken](cognitive-services-recommendations-buildtypes.md).

1. Klik op de knop **Opbouwen** . Het proces opbouwen duurt ongeveer 5 minuten als u de gegevens boeken gebruikt. Het duurt langer op grotere gegevenssets.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Stap 5: we weten wat de computer hebt geleerd! ##

Wanneer uw opbouwen is voltooid, ziet u een nieuwe build in de lijst builds. Deze build kan worden doorzocht voor aanbevelingen van item en de gebruiker.

1. Wanneer uw opbouwen is voltooid, klikt u op **Score**. Hiermee kunt u met het model afspelen en ziet u welke items worden aanbevolen.

    ![Aanbevelingen UI: Knop van de Score][reco_score_button]

1. Selecteer een item om te zien welke items worden geretourneerd als aanbevelingen voor dat item. Zoals u ziet dat als er niet genoeg transacties om te voorspellen een aanbeveling voor een bepaald item, het systeem niet eventuele aanbevelingen voor dat item retourneert.  Als om een bepaalde reden u een item dat er geen aanbevelingen geeft hebt, kunt u andere items scoren.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Stap 6 - Vervolgstappen ##
Gefeliciteerd! u hebt een model training en vervolgens aanbevelingen van dat model hebt gekregen.  De gebruikersinterface aanbevelingen is een handig hulpmiddel waarmee u kunt de status van uw projecten zien en genereert. 

Nu dat u een model hebt, is het raadzaam om te leren hoe u de bovenstaande stappen via programmacode. Om te weten de API via programmacode bellen, uitnodigen u kunt controleren van de [Aanbevelingen API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) en de [Aanbevelingen steekproef-toepassing](http://go.microsoft.com/fwlink/?LinkID=759344)te downloaden.


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
