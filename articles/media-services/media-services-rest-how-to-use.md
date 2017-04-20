<properties 
    pageTitle="Overzicht van de Media Services REST API | Microsoft Azure" 
    description="Overzicht van de Media Services REST API" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Overzicht van de Media Services REST API 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure Media Services is een service die op basis van OData HTTP-aanvragen accepteert en weer aan uitgebreide JSON of atom + Publisher kan reageren. Omdat Media Services aan de richtlijnen voor het ontwerpen van Azure voldoen, is er een verzameling vereiste HTTP-headers dat elke client gebruiken moet als u verbinding maakt met Media Services, evenals een reeks optioneel koppen die kunnen worden gebruikt. De volgende secties worden de koppen en HTTP-woorden die u kunt gebruiken bij het maken van aanvragen en antwoorden van Media Services ontvangt.

##<a name="considerations"></a>Overwegingen 

Het volgende letten wanneer u een REST gebruikt.

- Query's naar entiteiten, moet u er een limiet van 1000 entiteiten in één keer wordt geretourneerd, omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt is. U moet gebruiken **overslaan** en **uitvoeren** (.NET) / **boven** (REST) als beschreven in [dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [in dit voorbeeld REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 

- Wanneer JSON en als u wilt gebruiken het trefwoord **__metadata** in het verzoek te geven (bijvoorbeeld naar verwijst naar een gekoppelde object) moet u de koptekst **accepteren** instellen naar [uitgebreide JSON-indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (Zie het volgende voorbeeld). De eigenschap **__metadata** in het verzoek, worden niet-OData begrepen tenzij u deze op uitgebreide instelt.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standaard HTTP-verzoek headers ondersteund door Services Media

Er is een verzameling vereiste kopteksten die u in uw verzoek opnemen moet voor elke oproep die u in Media Services aanbrengt, en ook een reeks optioneel kopteksten u mogelijk wilt opnemen. De volgende tabel bevat de vereiste koppen:


Koptekst|Type|Waarde
---|---|---
Autorisatie|Vruchtdragende|Vruchtdragende is de enige geaccepteerde autorisatie-methode. De waarde moet ook het toegangstoken verstrekt door ACS bevatten.
x-ms-versie|Decimaal|2.11
DataServiceVersion|Decimaal|3.0
MaxDataServiceVersion|Decimaal|3.0



>[AZURE.NOTE] Omdat OData Media Services gebruikt om weer te geven van de onderliggende opslag van de metagegevens van activa via REST API's, moeten de koppen DataServiceVersion en MaxDataServiceVersion worden opgenomen in elk verzoek; echter als dat niet het geval is, klikt u vervolgens momenteel Media Services wordt ervan uitgegaan de waarde DataServiceVersion in gebruik is 3.0.

Hieronder ziet een reeks optioneel kopteksten.

Koptekst|Type|Waarde
---|---|---
Datum|RFC 1123 datum|Tijdstempel van de aanvraag kunt invullen
Accepteren|Inhoudstype|De gevraagde inhoudstype voor het antwoord zoals de volgende handelingen uit:<p> -toepassing/json; odata = uitgebreide<p> -toepassing/atom + xml<p> Antwoorden mogelijk een ander type inhoud, zoals een ophalen blob waar een succesvolle antwoord de stroom blob als de nettolading bevat.
Accepteren-codering|Gzip, verkleinen|GZIP en DEFLATE codering, indien van toepassing. Opmerking: Voor grote resources, Media Services mogelijk deze koptekst negeren en levert gegevens retourneren.
Accepteren-taal|'en', 'en', enzovoort.|Hiermee geeft u de gewenste taal voor het antwoord.
Accepteren-tekenset|Tekenset type zoals "UTF-8"|Standaard is UTF-8.
X-HTTP-methode|HTTP-methode|Hiermee kan clients of firewalls die HTTP-methoden zoals opslag of verwijderen voor het gebruik van deze methoden tunnel via een GET-gesprek niet ondersteunen.
Inhoudstype|Inhoudstype|Inhoudstype van het hoofdgedeelte van de aanvraag in opslag of POST aanvragen.
client-aanvraag-id|Tekenreeks|Een door de beller gedefinieerde waarde die het opgegeven verzoek aangeeft. Als u opgeeft, wordt deze waarde in het antwoordbericht opgenomen om toe te wijzen het verzoek. <p><p>**Belangrijke**<p>Waarden moeten worden beperkt tot 2096b (2k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standaard HTTP-antwoordheaders ondersteund door Services Media

Hierna volgt een aantal koppen die kunnen worden geretourneerd voor u, afhankelijk van de resource die u zijn aangevraagd en de actie die u bedoeld om uit te voeren.


Koptekst|Type|Waarde
---|---|---
aanvraag-id|Tekenreeks|Een unieke id voor de huidige bewerking, service gegenereerd.
client-aanvraag-id|Tekenreeks|Een id die is opgegeven door de beller in de oorspronkelijke uitnodiging, als deze aanwezig zijn.
Datum|RFC 1123 datum|De datum waarop de aanvraag is verwerkt.
Inhoudstype|Varieert|Het type inhoud van de hoofdtekst van het antwoord.
Inhoud-codering|Varieert|Gzip of verkleinen, passende.


## <a name="standard-http-verbs-supported-by-media-services"></a>Standaard HTTP-woorden die worden ondersteund door Services Media

Hier volgt een volledige lijst van HTTP-woorden die kunnen worden gebruikt als HTTP waardoor aanvraagt:


Werkwoord|Beschrijving
---|---
Toevoegen|Geeft als resultaat de huidige waarde van een object.
Verzenden|Hiermee maakt u een object op basis van de gegevens die zijn opgegeven, of een opdracht ingediend.
PLAATSEN|Hiermee vervangt u een object of Hiermee maakt u een benoemd object (indien van toepassing).
VERWIJDEREN|Hiermee verwijdert u een object.
SAMENVOEGEN|Een bestaand object bijgewerkt met benoemde eigenschapswijzigingen.
HOOFD|Geeft als resultaat de metagegevens van een object voor een GET-reactie.

##<a name="limitation"></a>Beperking

Query's naar entiteiten, moet u er een limiet van 1000 entiteiten in één keer wordt geretourneerd, omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt is. U moet gebruiken **overslaan** en **uitvoeren** (.NET) / **boven** (REST) als beschreven in [dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [in dit voorbeeld REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 


## <a name="discovering-media-services-model"></a>Media Services model ontdekken

Als u Media Services entiteiten overzichtelijker, kan de bewerking $metadata worden gebruikt. U kunt alle typen geldige entiteit, entiteitseigenschappen, koppelingen, functies, acties en dergelijke ophalen. Het volgende voorbeeld ziet u het maken van de URI: https://media.windows.net/API/$ metagegevens.

U moet toevoegen '? api-version=2.x "naar het einde van de URI als u wilt weergeven van de metagegevens in een browser of de kop van de x-ms-versie niet in uw aanvraag kunt invullen opnemen.



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 
