<properties
    pageTitle="JSON-uitvoer van Stream analysegegevens | Microsoft Azure"
    description="Leer hoe Stream Analytics Azure DocumentDB voor JSON-uitvoer voor gegevens archiveren en lage latentie query's op ongestructureerde JSON gegevens kunt afstemmen."
    keywords="JSON-uitvoer"
    documentationCenter=""
    services="stream-analytics,documentdb"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>TARGET Azure DocumentDB voor JSON-uitvoer van Stream Analytics

Stream Analytics kunt [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) afstemmen voor JSON uitvoert, inschakelen en archiveren lage latentie gegevensquery op ongestructureerde JSON-gegevens. Leer hoe u het beste implementeren deze integratie.

Voor mensen die niet bekend met DocumentDB bent, raadpleegt u [leerpad van DocumentDB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) aan de slag.

## <a name="basics-of-documentdb-as-an-output-target"></a>Basisprincipes van DocumentDB als een uitvoerdoel
De uitvoer Azure DocumentDB in Stream Analytics kunt schrijven uw resultaten als JSON-uitvoer verwerken in uw collection(s) DocumentDB stream. Stream Analytics maakt geen verzamelingen in uw database, waarvoor u ze voorhand maken in plaats hiervan is vereist. Dit is dat de facturering kosten van DocumentDB collecties transparant voor u zijn en dat kunt u de prestaties, de consistentie en de capaciteit van uw siteverzamelingen rechtstreeks met de [API's DocumentDB](https://msdn.microsoft.com/library/azure/dn781481.aspx)afstemmen. Het is raadzaam om één DocumentDB Database per streaming project gebruikt zodat u kunt uw verzamelingen voor een taak streaming logisch te scheiden.

Enkele van de opties van de siteverzameling DocumentDB worden hieronder beschreven.

## <a name="tune-consistency-availability-and-latency"></a>Consistentie, beschikbaarheid en latentie afstemmen

Zodat deze overeenkomen met uw toepassingsvereisten voor de, kunt DocumentDB u deze af te stellen van de Database en de collecties en brengt u voor-en nadelen tussen consistentie, beschikbaarheid en latentie. Afhankelijk van welke detailniveaus gelezen consistentie de behoeften van uw scenario tegen lezen en schrijven latentie, dat kunt u een niveau consistentie van uw databaseaccount. Standaard kunt DocumentDB ook synchroon indexeren op elke CRUD-bewerkingen aan uw verzameling. Dit is een andere handige optie om te bepalen de prestaties schrijven/lezen in DocumentDB. Lees het [wijzigen van uw database en query consistentie niveaus](../documentdb/documentdb-consistency-levels.md) -artikel voor meer informatie over dit onderwerp.

## <a name="choose-a-performance-level"></a>Kies het machtigingsniveau dat prestaties

DocumentDB verzamelingen kunnen worden gemaakt op 3 van verschillende prestatieniveaus (S1, S2 of S3), die bepalen van de beschikbare doorvoer voor CRUDs aan die verzameling. Daarnaast kunnen wordt prestaties beïnvloed door de machtigingsniveaus indexeren/consistentie van uw siteverzameling. Zie [dit artikel](../documentdb/documentdb-performance-levels.md) voor informatie over deze prestaties uitgebreid beschreven.

## <a name="upserts-from-stream-analytics"></a>Upserts van Stream analyseprogramma

Stream Analytics-integratie met DocumentDB kunt u invoegen of bijwerken van records in de verzameling DocumentDB is gebaseerd op een bepaald Document-ID-kolom. Dit is ook een *Upsert*genoemd.

Stream Analytics maakt gebruik van een optimistische Upsert benadering, waar de updates zijn alleen Gereed wanneer invoegen vanwege een conflict Document-ID mislukt. Deze update wordt uitgevoerd door Stream Analytics als een PATCH, zodat u kunt gedeeltelijke updates naar het document, dat wil zeggen de toevoeging van nieuwe eigenschappen of het vervangen van die een bestaande eigenschap stapsgewijs wordt uitgevoerd. Houd er rekening mee dat wijzigingen in de waarden van de Matrixeigenschappen in het resultaat van het document JSON in de hele matrix ophalen overschreven, dat wil zeggen de matrix niet is samengevoegd.

## <a name="data-partitioning-in-documentdb"></a>Gegevens in DocumentDB partitioneren

DocumentDB verzamelingen kunnen u uw gegevens op basis van de query patronen en de prestatiebehoeften van uw toepassing partitioneren. Elke siteverzameling kan maximaal 10GB gegevens (maximaal) bevatten en er is momenteel geen manier om een verzameling vergroten (of overloop). Voor schalen, Stream analyses, kunt u schrijven naar meerdere siteverzamelingen met een gegeven voorvoegsel (Zie onderstaande details van gebruik). Stream Analytics wordt de consistente [Hash Partition resolvercache](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) strategie op basis van de gebruiker opgegeven PartitionKey kolom om de records uitvoer partitioneren. Het aantal collecties met het opgegeven voorvoegsel tijdens het starten van een van de streaming taak wordt gebruikt als het aantal uitvoer partities, waaraan de taak gegevens worden geschreven naar parallel (DocumentDB verzamelingen uitvoer partities =). Voor een enkele S3 siteverzameling met fikse indexing doen alleen ingevoegd, over 0,4 MB/s schrijfbewerkingen kan worden verwacht. Meerdere siteverzamelingen gebruiken, kunt u sneller worden verwerkt en verhoogde capaciteit bereiken.

Als u van plan bent om uit te breiden het aantal partities in de toekomst, moet u mogelijk uw taak stoppen, de gegevens uit uw bestaande collecties naar nieuwe collecties partitioneren en de Stream Analytics-taak vervolgens opnieuw te starten. Meer informatie over het gebruik van PartitionResolver en opnieuw partitioneren samen met het voorbeeld van code, worden opgenomen in een bericht voor opvolging. De artikel [partitioneren in DocumentDB](../articles/documentdb-partition-data.md#developing-a-partitioned-application) bevat, schakelt u ook informatie hierover in.

## <a name="documentdb-settings-for-json-output"></a>DocumentDB-instellingen voor JSON-uitvoer

DocumentDB maken als een uitvoer in Stream Analytics genereert een herinnering voor informatie zoals hieronder wordt weergegeven. Deze sectie bevat een uitleg van de definitie eigenschappen.

![documentdb stream analytics uitvoerscherm](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Uitvoeralias** – een alias om te verwijzen deze uitvoer in uw query ASA  
-   **Accountnaam** : de naam of het eindpunt URI van het account DocumentDB.  
-   **Accountsleutel** – de gedeelde toegangstoets voor het account DocumentDB.  
-   **Database** – de DocumentDB databasenaam.  
-   **Siteverzameling naampatroon** – het patroon van de siteverzameling naam voor de verzamelingen moet worden gebruikt. De indeling van de siteverzameling worden gemaakt volgens de token optioneel {partition}, waar partities starten vanuit 0. Voorbeeld geldige invoer Hier volgen:  
   1\) MyCollection – een siteverzameling met de naam 'MyCollection' moet bestaan.  
   2\) MyCollection {partition} – deze verzamelingen moeten bestaan – "MyCollection0", "MyCollection1", "MyCollection2", enzovoort.  
-   **Partition toets** – de naam van het veld in de uitvoer gebeurtenissen gebruikt de sleutel voor uitvoer partitioneren over verzamelingen te geven. Voor één siteverzameling uitvoer, een uitvoerkolom willekeurige kan zijn bijvoorbeeld PartitionId gebruikt.  
-   **Document-ID** : optioneel. De naam van het veld in de uitvoer gebeurtenissen die worden gebruikt om op te geven van de primaire sleutel welke invoegen of bijwerken bewerkingen zijn gebaseerd.  
