<properties
    pageTitle="Bij gelijktijdigheid in Microsoft Azure-opslag beheren"
    description="Het beheren van gelijktijdigheid voor de services Blob, wachtrij tabel en bestand"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Bij gelijktijdigheid in Microsoft Azure-opslag beheren

## <a name="overview"></a>Overzicht

Moderne Internet op basis van toepassingen hebben meestal meerdere gebruikers weergeven en gegevens tegelijk bijwerken. Hiervoor is vereist voor ontwikkelaars van toepassingen goed bedenken hoe u een overzichtelijk om ervaring te bieden hun eindgebruikers, met name voor scenario's waarin meerdere gebruikers dezelfde gegevens kunnen bijwerken. Er zijn drie belangrijke gegevens bij gelijktijdigheidsstrategieën ontwikkelaars zullen meestal overwegen:  


1.  Optimistische gelijktijdigheid – een toepassing uitvoering van die een update als onderdeel van de update verifiëren wordt als de gegevens zijn gewijzigd sinds de toepassing die gegevens laatst lezen. Bijvoorbeeld als twee gebruikers weergeven van een wikipagina maakt een update naar dezelfde pagina klikt u vervolgens het wiki-platform moet ervoor zorgen dat de tweede update wordt niet overschreven door de eerste update – en dat beide gebruikers begrijpen of hun update is geslaagd of mislukt. Deze strategie wordt meestal gebruikt in webtoepassingen.
2.  Pessimistische gelijktijdigheid – het vinden van een toepassing als u wilt uitvoeren van een update wordt pas van een slot op een object voorkomen dat andere gebruikers bijwerken van de gegevens totdat de vergrendeling wordt opgeheven. Bijvoorbeeld in een hoofdsectie/detailsectie gegevens herhaling scenario waarbij alleen de hoofdlijst updates wordt uitgevoerd wordt de basispagina meestal houdt een exclusieve vergrendeling voor langere tijd op de gegevens om ervoor te zorgen dat er niemand anders kan worden bijgewerkt.
3.  Laatste schrijver wins – werkt het toestaan van alle updatebewerkingen te gaan zonder te verifiëren als andere toepassingen heeft bijgewerkt de gegevens sinds de toepassing eerst de gegevens lezen. Deze strategie (of gebrek aan een formele strategie) wordt meestal gebruikt waarin gegevens is partitioneren zodanig dat er geen waarschijnlijkheid is dat meerdere gebruikers toegang dezelfde gegevens tot. Het kan ook handig zijn waar tijdelijk gegevensstromen worden verwerkt.  

Dit artikel bevat een overzicht van hoe de opslag van Azure platform vergemakkelijkt de ontwikkeling eerste-klas door ondersteuning te bieden voor alle drie deze gelijktijdigheidsstrategieën.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure opslag – vergemakkelijkt de ontwikkeling van de Cloud
De Azure storage-service ondersteuning biedt voor alle drie strategieën, hoewel het opvallend in de mogelijkheid om aan te bieden volledige ondersteuning voor optimistische en Pessimistische gelijktijdigheid omdat deze is ontworpen voor Profiteer van een sterke consistentie model die zorgt u ervoor dat wanneer de Storage-service doorgevoerd een gegevens invoegen of de bijwerkbewerking alle verder toegang heeft tot aan dat gegevens te krijgen de meest recente update zien. Opslagplatforms die gebruikmaken van een model eventuele consistentie hebben een vertraging tussen wanneer een schrijven wordt uitgevoerd door één gebruiker en wanneer de bijgewerkte gegevens zijn zichtbaar voor andere gebruikers dus complicerende ontwikkeling van clienttoepassingen om te voorkomen dat inconsistenties eindgebruikers beïnvloeden.  

Naast het selecteren van een juiste gelijktijdigheidsstrategie moeten ontwikkelaars zich op de hoogte van hoe een opslag-platform wijzigingen – met name wijzigingen aan hetzelfde object over transacties geïsoleerd. De Azure storage-service wordt overgeslagen toe te staan dat gelezen bewerkingen optreden als schrijven bewerkingen binnen een enkel partition gebruikt. In tegenstelling tot andere niveaus moeten worden geïsoleerd overgeslagen zorgt ervoor dat alle lees een consistente momentopname van de gegevens zichtbaar, zelfs als er updates zijn optreedt – in principe door te retourneren van het laatste vastgelegde waarden tijdens een update transactie wordt verwerkt.  

## <a name="managing-concurrency-in-blob-storage"></a>Bij gelijktijdigheid beheren in Blob storage
U kunt kiezen om een van beide modellen optimistische of Pessimistische gelijktijdigheid gebruiken voor het beheren van toegang tot BLOB's en containers in de blob-service. Als u een strategie laatste schrijft wins niet expliciet opgeeft is de standaardwaarde.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistische gelijktijdigheid voor BLOB's en containers  

De Storage-service wordt een id toegewezen aan elk object dat is opgeslagen. Deze id wordt bijgewerkt telkens wanneer een updatebewerking wordt uitgevoerd op een object. De id geretourneerd naar de client als onderdeel van een HTTP GET antwoord wordt verzonden met de koptekst van ETag (entiteit tag) die is gedefinieerd in het HTTP-protocol. Een gebruiker een update op dergelijk object uit te voeren kunt verzenden in de oorspronkelijke ETag samen met de kop van een voorwaardelijke om ervoor te zorgen dat een update vindt alleen plaats als een bepaalde voorwaarde is voldaan: in dit geval de voorwaarde is een 'If-Match' kop waarvoor de Storage-Service om ervoor te zorgen de waarde van de opgegeven in de updateaanvraag ETag is dezelfde als die opgeslagen in de Storage-Service.  

Het overzicht van dit proces is als volgt:  

1.  Een blob halen uit de storage-service, het antwoord bevat een HTTP ETag koptekst-waarde die aangeeft van de huidige versie van het object in de storage-service.
2.  Wanneer u de blob bijwerkt, voegt u de ETag-waarde die u in stap 1 in de **If-Match** voorwaardelijke koptekst van de aanvraag die u met de service verzendt hebt ontvangen.
3.  De service vergeleken de ETag-waarde in het verzoek met de huidige ETag-waarde van de blob.
4.  Als de huidige ETag-waarde van de blob een andere versie dan de ETag in de kop in het verzoek tot een **If-Match** voorwaardelijke staat is, retourneert de service een 412 fout naar de klant. Hiermee geeft u aan de client dat een ander proces de blob heeft bijgewerkt nadat de client opgehaald deze.
5.  Als de huidige ETag-waarde van de blob dezelfde versie als de ETag in de kop in het verzoek tot een **If-Match** voorwaardelijke staat is, wordt de service kunt u de bewerking uitvoeren en updates van de huidige ETag-waarde van de label om weer te geven dat dit een nieuwe versie heeft gemaakt.  

Het volgende C#-fragment (via de Client opslag bibliotheek 4.2.0) ziet u een eenvoudige voorbeeld van het maken van een **If-Match AccessCondition** op basis van de waarde van ETag dat wordt geopend vanuit de eigenschappen van een blob die is eerder opgehaald of ingevoegd. Vervolgens wordt het object **AccessCondition** wanneer het bijwerken van de blob: het object **AccessCondition** wordt de koptekst **If-Match** toegevoegd aan het verzoek. Wanneer de blob is bijgewerkt naar een ander proces, retourneert de service blob een statusbericht HTTP 412 (voorwaarde is mislukt). U kunt hier de volledige steekproef downloaden: [Bij gelijktijdigheid met Azure opslag beheren](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

De Storage-Service bevat ook ondersteuning voor extra voorwaardelijke kopteksten zoals **Als gewijzigd sinds**, **Als ongewijzigd sinds** en **If-None-Match** , evenals combinaties daarvan. Zie [Voorwaardelijke kopteksten opgeven voor Blob servicebewerkingen](http://msdn.microsoft.com/library/azure/dd179371.aspx) op MSDN voor meer informatie.  

De volgende tabel bevat een overzicht van de container bewerkingen die voorwaardelijke kopteksten zoals **If-Match** in het verzoek accepteren en die een ETag-waarde in het antwoord te retourneren.  

| Bewerking                | Geeft als resultaat de Container ETag-waarde | Voorwaardelijke kopteksten accepteert |
|:-------------------------|:-----------------------------|:----------------------------|
| Container maken         | Ja                          | Nee                          |
| Containereigenschappen van de ophalen | Ja                          | Nee                          |
| Container metagegevens ophalen   | Ja                          | Nee                          |
| Metagegevens van de Container instellen   | Ja                          | Ja                         |
| Container ACL ophalen        | Ja                          | Nee                          |
| Container ACL instellen        | Ja                          | Ja (*)                     |
| Container verwijderen         | Nee                           | Ja                         |
| Lease Container          | Ja                          | Ja                         |
| Lijst BLOB 's               | Nee                           | Nee                          |

(*) De machtigingen die zijn gedefinieerd door SetContainerACL in cache opslaan en updates van deze machtigingen 30 seconden voordat in welke periode updates zijn niet gegarandeerd consistente duren.  

De volgende tabel worden de blob-bewerkingen die voorwaardelijke kopteksten zoals **If-Match** in het verzoek accepteren en die een ETag-waarde in het antwoord te retourneren.

| Bewerking           | Geeft als resultaat de waarde ETag | Voorwaardelijke kopteksten accepteert           |
|:--------------------|:-------------------|:--------------------------------------|
| Blob plaatsen            | Ja                | Ja                                   |
| Blob ophalen            | Ja                | Ja                                   |
| Eigenschappen van Blob ophalen | Ja                | Ja                                   |
| Blob-eigenschappen instellen | Ja                | Ja                                   |
| Blob metagegevens ophalen   | Ja                | Ja                                   |
| Blob metagegevens instellen   | Ja                | Ja                                   |
| Lease Blob (*)      | Ja                | Ja                                   |
| Momentopname Blob       | Ja                | Ja                                   |
| Blob kopiëren           | Ja                | Ja (voor de bron- en doeltabellen blob) |
| Kopie Blob afbreken     | Nee                 | Nee                                    |
| Blob verwijderen         | Nee                 | Ja                                   |
| Blok plaatsen           | Nee                 | Nee                                    |
| Lijst met geblokkeerde plaatsen      | Ja                | Ja                                   |
| Lijst met geblokkeerde ophalen      | Ja                | Nee                                    |
| Pagina plaatsen            | Ja                | Ja                                   |
| Get-paginabereik     | Ja                | Ja                                   |


(*) Lease Blob ongewijzigd de ETag op een blob blijven.  

### <a name="pessimistic-concurrency-for-blobs"></a>Pessimistische gelijktijdigheid voor BLOB 's
Als u wilt vergrendelen een blob voor exclusief gebruik, kunt u een [lease](http://msdn.microsoft.com/library/azure/ee691972.aspx) erop aanschaffen. Als u een lease aanschaft, kunt u opgeven hoe lang moet u de lease: dit is voor tussen 15 tot 60 seconden of oneindig waarin bedragen exclusief te wijzigen. U kunt een eindige lease te laten verlengen en u kunt elke lease vrijgeven wanneer u klaar met deze bent. De service blob loslaat automatisch eindige lease wanneer ze verlopen.  

Lease inschakelen andere synchronisatie strategieën ondersteund, met inbegrip van exclusief schrijven / shared gelezen, exclusieve schrijven / exclusieve lezen en schrijven gedeeld / exclusieve lezen. Wanneer een lease bestaat de storage-service wordt afgedwongen exclusieve schrijft (plaatsen, instellen en verwijderingen) ervoor zorgen dat exclusiviteit voor meer bewerkingen moet echter de ontwikkelaar om ervoor te zorgen dat alle clienttoepassingen een lease-ID en die slechts één client tegelijk gebruiken heeft een geldige lease-ID. Lees de bewerkingen die het resultaat van een lease-ID niet in gedeelde lezen opnemen.  

Het volgende C#-fragment toont een voorbeeld van bij het verkrijgen van een exclusief lease 30 seconden ingedrukt op een blob en vervolgens de lease los te laten bijwerken van de inhoud van de blob. Als er al een geldige lease op de blob wanneer u probeert te verkrijgen van een nieuwe lease, retourneert de service blob een resultaat van de status "HTTP (409) Conflict". Het codefragment van de onderstaande gebruikmaakt van een object **AccessCondition** naar de lease-informatie onderbrengen waarmee een aanvraag voor het bijwerken van de label in de storage-service.  U kunt hier de volledige steekproef downloaden: [Bij gelijktijdigheid met Azure opslag beheren](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Als u een bewerking schrijven op een lease blob probeert zonder het doorgeven van de lease-ID, mislukt het verzoek met een 412 fout. Houd er rekening mee dat als de lease verloopt voordat u de methode **UploadText** , maar u nog steeds de ID van de lease geven, de aanvraag ook met een fout **412 mislukt** . Zie de documentatie van de REST [Lease Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) voor meer informatie over het beheren van lease verstrijken tijden en lease-id's.  

De volgende blob bewerkingen kunnen lease gebruiken voor het beheren van Pessimistische gelijktijdigheid:  


-   Blob plaatsen
-   Blob ophalen
-   Eigenschappen van Blob ophalen
-   Blob-eigenschappen instellen
-   Blob metagegevens ophalen
-   Blob metagegevens instellen
-   Blob verwijderen
-   Blok plaatsen
-   Lijst met geblokkeerde plaatsen
-   Lijst met geblokkeerde ophalen
-   Pagina plaatsen
-   Get-paginabereik
-   Momentopname Blob - lease-ID optioneel als een lease bestaat
-   Blob - ID vereist als een lease op de bestemming blob voorkomt lease kopiëren
-   Afbreken kopie Blob - lease ID vereist als een oneindig lease op de bestemming blob voorkomt
-   Lease Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Pessimistische gelijktijdigheid voor containers
Lease op containers de dezelfde synchronisatie strategieën ondersteund als u in de BLOB's inschakelen (exclusief schrijven / shared gelezen, exclusieve schrijven / exclusieve lezen en schrijven gedeeld / exclusieve lezen) echter in tegenstelling tot BLOB's de storage-service alleen worden toegepast exclusiviteit op verwijderen. Als u wilt verwijderen in een container met een actieve lease, moet een client met het verzoek verwijderen om de ID van de actieve lease bevatten. Alle andere bewerkingen uit de container mislukt op een lease container zonder zoals de ID lease bewerkingen in dat geval ze zijn gedeeld. Als exclusiviteit van bijwerken (opslag of set) of lees bewerkingen vereist is vervolgens ontwikkelaars moeten ervoor zorgen dat alle clients gebruiken een lease-ID en dat er slechts één client tegelijk een geldige lease-ID.  

De volgende bewerkingen in de container kunnen lease gebruiken voor het beheren van Pessimistische gelijktijdigheid:  

-   Container verwijderen
-   Containereigenschappen van de ophalen
-   Container metagegevens ophalen
-   Metagegevens van de Container instellen
-   Container ACL ophalen
-   Container ACL instellen
-   Lease Container  

Zie voor meer informatie:  

- [Voorwaardelijke kopteksten opgeven voor Blob servicebewerkingen](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Lease Container](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Lease Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Bij gelijktijdigheid in de tabel-Service beheren
De tabel-service gebruikt optimistische gelijktijdigheid gecontroleerd als zich standaard gedraagt wanneer u werkt met entiteiten, in tegenstelling tot de blob-service waarin u expliciet kiezen moet optimistische gelijktijdigheid controles uit te voeren. De andere verschil tussen de tabel en blob services is dat u alleen het gedrag bij gelijktijdigheid van entiteiten beheren kunt terwijl u met de service blob kunt u de gelijktijdigheid van zowel containers en BLOB's beheren.  

Gebruik van optimistische gelijktijdigheid en om te controleren of een ander proces een entiteit gewijzigd sinds u deze uit de tabel storage-service opgehaald, kunt u de ETag-waarde die u ontvangt wanneer de tabel-service een entiteit retourneert. Het overzicht van dit proces is als volgt:  

1.  Een entiteit halen uit de tabel storage-service, het antwoord bevat een ETag-waarde die aangeeft van de huidige identificatie die is gekoppeld aan die entiteit in de storage-service.
2.  Wanneer u de entiteit bijwerkt, voegt u de ETag-waarde die u in stap 1 in de verplicht **If-Match** koptekst van de aanvraag die u met de service verzendt hebt ontvangen.
3.  De service vergeleken de ETag-waarde in het verzoek met de huidige ETag-waarde van de entiteit.
4.  Als de huidige ETag-waarde van de entiteit verschilt van de ETag in de verplicht **If-Match** koptekst in het verzoek, retourneert de service een 412 fout naar de klant. Hiermee geeft u aan de client dat een ander proces de entiteit heeft bijgewerkt nadat de client opgehaald deze.
5.  Als de huidige ETag-waarde van de entiteit hetzelfde als de ETag in de verplicht **If-Match** kop in het verzoek is of de kop van de **If-Match** bevat de jokerteken (*), wordt de service kunt u de bewerking uitvoeren en updates van de huidige ETag-waarde van de entiteit om weer te geven dat deze is bijgewerkt.  

Houd er rekening mee dat in tegenstelling tot de service blob, de tabel-service is vereist de client naar de kop van een **If-Match** opnemen in verzoeken om te werken. Het is echter mogelijk is om af te dwingen een onvoorwaardelijke bijwerken (laatste schrijver WINS-strategie) en gelijktijdigheid controles overslaan als de client Hiermee stelt u de kop van het **If-Match** op het jokerteken (*) in het verzoek.  

Het volgende C#-fragment ziet u een klantentiteit die eerder hebt gemaakt of opgehaald met hun e-mailadres dat is bijgewerkt. De oorspronkelijke invoegen of op te halen bewerking winkels ETag-waarde in het object klant omdat het voorbeeld wordt hetzelfde objectexemplaar gebruikt bij het uitvoeren van de vervangbewerking, verzonden en wordt automatisch de waarde ETag terug naar de tabel-service, zodat de service controleren voor gelijktijdigheid conflicten. Als een ander proces de entiteit in tabelopslag bijgewerkt heeft, retourneert de service een statusbericht HTTP 412 (voorwaarde is mislukt).  U kunt hier de volledige steekproef downloaden: [Bij gelijktijdigheid met Azure opslag beheren](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Als u wilt uitschakelen expliciet het selectievakje bij gelijktijdigheid, moet u de eigenschap **ETag** van het object **werknemer** instellen "*" voordat u de vervangbewerking uitvoert.  

    customer.ETag = "*";  

De volgende tabel ziet hoe de tabel entity-bewerkingen ETag waarden gebruiken:

| Bewerking                | Geeft als resultaat de waarde ETag | If-Match verzoek koptekst vereist |
|:-------------------------|:-------------------|:---------------------------------|
| Query entiteiten           | Ja                | Nee                               |
| Entiteit invoegen            | Ja                | Nee                               |
| Entiteit bijwerken            | Ja                | Ja                              |
| Entiteit samenvoegen             | Ja                | Ja                              |
| Entiteit verwijderen            | Nee                 | Ja                              |
| Invoegen of entiteit vervangen | Ja                | Nee                               |
| Invoegen of entiteit samenvoegen   | Ja                | Nee                               |

Opmerking die de bewerkingen **invoegen of entiteit vervangen** en **invoegen of entiteit samenvoegen** uitvoeren *niet* alle controles bij gelijktijdigheid uitvoeren, omdat ze Stuur geen een ETag-waarde naar de tabel-service.  

In het algemeen ontwikkelaars met behulp van tabellen moeten zijn afhankelijk van de optimistische gelijktijdigheid bij het ontwikkelen van scalable toepassingen. Als volledige vergrendeling nodig is, worden één methode ontwikkelaars wanneer het openen van tabellen is toewijzen van een aangewezen blob voor elke tabel en probeert te ondernemen een lease de blob voordat het besturingssysteem op de tabel. Deze methode is de toepassing om te zorgen dat alle gegevens access paden de lease voordat u op de tabel werkzaam aanvragen vereist. Vergeet niet dat de minimale leasetijd is 15 seconden die is vereist dat u daarbij aandacht voor schaalbaarheid.  

Zie voor meer informatie:  

- [Bewerkingen op entiteiten](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Bij gelijktijdigheid in de wachtrijservice beheren
Een scenario die in welke gelijktijdigheid een probleem in de wachtrij-service is is waar meerdere klanten berichten ophaalt uit een wachtrij. Wanneer een bericht wordt opgehaald uit de wachtrij, wordt het antwoord bevat het bericht en een pop ontvangstwaarde, die is vereist voor het verwijderen van het bericht. Het bericht wordt niet automatisch verwijderd uit de wachtrij, maar nadat deze is opgehaald, het is niet zichtbaar voor andere clients voor het tijdsinterval die is opgegeven met de parameter visibilitytimeout. De client die het bericht haalt naar verwachting het bericht wilt verwijderen nadat deze is verwerkt en voordat u de tijd die is opgegeven door de TimeNextVisible element van het antwoord, die wordt berekend op basis van de waarde van de parameter visibilitytimeout. De waarde van visibilitytimeout wordt toegevoegd aan het tijdstip waarop het bericht om te bepalen de waarde van TimeNextVisible zijn opgehaald.  

De wachtrijservice biedt geen ondersteuning voor optimistische of Pessimistische gelijktijdigheid en hiervoor reden clients afhandeling van berichten die zijn opgehaald uit een wachtrij moeten zorgen dat de berichten worden verwerkt in een wijze idempotency is ingeschakeld. Een laatste schrijver WINS-strategie wordt gebruikt voor het bijwerken van gegevens zoals SetQueueServiceProperties, SetQueueMetaData, SetQueueACL en UpdateMessage.  

Zie voor meer informatie:  

- [Wachtrij Service REST API](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Berichten ontvangt](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Gelijktijdigheid in het bestand-Service beheren
De service bestand kan worden geopend met twee verschillende protocoleindpunten – SMB en houd. De REST-service biedt geen ondersteuning voor het beperkte vergrendeling of pessimistische vergrendelen en alle updates volgen nog een laatste schrijver WINS-strategie. SMB-clients die bestandsshares koppelen kunnen gebruikmaken van bestand systeem vergrendelingsmechanismen om toegang tot gedeelde bestanden, waaronder de mogelijkheid om uit te voeren volledige vergrendeling beheren. Wanneer een SMB-client een bestand opent, deze Hiermee geeft u de toegang tot bestanden en de delen modus. Instellen van een optie bestandstoegang van 'Schrijven' of 'Alleen-lezen/schrijven' samen met een bestandsshare-modus, "Geen" resulteert in het bestand wordt vergrendeld door een SMB-client totdat het bestand wordt gesloten. De REST-service retourneert statuscode 409 (Conflict) met foutcode SharingViolation als REST bewerking wordt uitgevoerd voor een bestand waar een SMB-client het bestand is vergrendeld heeft.  

Wanneer een SMB-client wordt geopend een bestand voor verwijderen, wordt het bestand gemarkeerd als wachtende verwijderen tot alle andere SMB-client openen om het formaat van dat bestand zijn gesloten. Terwijl een bestand is gemarkeerd als in behandeling verwijderen, kan elke willekeurige REST-bewerking op dat bestand statuscode 409 (Conflict) met foutcode SMBDeletePending zullen retourneren. Statuscode 404 (niet gevonden) is niet geretourneerd omdat het kan zijn voor het SMB-client om de verwijdering in behandeling markering vóór het sluiten van het bestand te verwijderen. Statuscode 404 (niet gevonden) verwachting met andere woorden, alleen wanneer het bestand is verwijderd. Houd er rekening mee dat wanneer een bestand in een SMB in behandeling de status verwijderen, deze niet in de lijst met bestanden met zoekresultaten opgenomen worden. Bedenk ook dat de REST verwijderen bestands- en REST verwijderen Directory-bewerkingen vastgelegde atomically en niet in in behandeling de status verwijderen resulteren.  

Zie voor meer informatie:  

- [Beheer van bestand vergrendelen](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Samenvatting en de volgende stappen
De Microsoft Azure Storage-service is ontworpen om te voldoen aan de behoeften van de meest complexe online toepassingen zonder ontwikkelaars manipuleren of belangrijke ontwerp hypothesen zoals gelijktijdigheid en gegevens consistentie die ze zich voordoen hebt te bedenken.  

Voor de volledige steekproef-toepassing waarnaar wordt verwezen in dit blog:  

- [Bij gelijktijdigheid met Azure Storage - steekproef-toepassing beheren](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Voor meer informatie over de opslag van Azure Zie:  

- [Introductiepagina van Microsoft Azure-opslag](https://azure.microsoft.com/services/storage/)
- [Inleiding tot Azure opslag](storage-introduction.md)
- Opslagruimte aan de slag voor [Blob](storage-dotnet-how-to-use-blobs.md), [tabel](storage-dotnet-how-to-use-tables.md), [wachtrijen](storage-dotnet-how-to-use-queues.md)en [bestanden](storage-dotnet-how-to-use-files.md)
- Opslagarchitectuur – [Azure opslag: een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
