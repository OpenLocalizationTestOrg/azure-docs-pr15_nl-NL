<properties 
    pageTitle="Inleiding tot Stream Analytics | Microsoft Azure" 
    description="Meer informatie over Stream Analytics, een beheerde service waarmee u streaming gegevens uit de Internet van dingen (IoT) analyseren in realtime." 
    keywords="Analytics beheerd als een service, services, stream verwerking, analytics-streaming, wat is stream analytics"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Wat is Stream Analytics?

Azure Stream Analytics is een volledig beheerde, kosten effectieve realtime gebeurtenis verwerking-engine die helpt bij het ontgrendelen van uitgebreide inzichten die gegevens. Stream Analytics kunt u gemakkelijk voor het instellen van realtime analytische berekeningen op gegevens streaming van apparaten, sensoren, websites, sociale media, -toepassingen, infrastructuursystemen en meer.

Met een paar muisklikken in de portal van Azure, kunt u een Stream Analytics-taak de invoerbron streaming gegevens, de uitvoer sink voor de resultaten van uw project precisie van de auteur en een gegevenstransformatie uitgedrukt in een SQL-achtige taal. U kunt controleren en pas de schaal/snelheid van uw taak in de portal van Azure aan de nieuwe schaal van een paar kilobytes naar een GB of meer van gebeurtenissen per seconde verwerkte.

Stream Analytics maakt gebruik van jaren van Microsoft Research werk bij het ontwikkelen van zeer afgestemd streaming-engines voor tijdgevoelig verwerking, maar ook taal-integraties voor intuïtieve specificaties van deze.

## <a name="what-can-i-use-stream-analytics-for"></a>Wat kan ik gebruiken Stream Analytics voor?
Grote hoeveelheden gegevens zijn die doorloopt met hoge snelheid via het netwerk vandaag. Organisaties die kunnen verwerken en reageren op deze streaming gegevens in realtime kunnen aanzienlijk efficiëntie te verhogen en zich in de onbepaalde te onderscheiden. Scenario's van realtime streaming analytics vindt u in alle sectoren: persoonlijke, realtime Aandelengrafiek handelsdag analyse en waarschuwingen die worden aangeboden door servicebedrijven financiële; realtime fraude detectie. beveiliging-services, gegevens en -identiteit; betrouwbare opname en analyseren van gegevens die zijn gegenereerd door sensoren en actuatoren ingesloten in fysiek objecten (Internet van zaken of IoT); clickstream webanalyse; en klant relatie management (CRM) toepassingen uitgeven waarschuwingen wanneer ervaring van de klant in een bepaald tijdsbestek is niet beschikbaar is weergegeven. Bedrijven zoekt de meest flexibele, betrouwbare en efficiënte manier om te doen zoals realtime gebeurtenis-stream gegevensanalyse te kunnen uitvoeren in de wereld ten zeerste Concurrentieanalyse moderne bedrijven.

## <a name="key-capabilities-and-benefits"></a>Belangrijke mogelijkheden en voordelen
-   **Gebruiksgemak:** Een eenvoudige, declaratieve query model ondersteunt stream Analytics voor de beschrijving van transformaties. Wilt optimaliseren voor gebruiksgemak, Stream Analytics een T-SQL-variant gebruikt en dat u voor klanten die u wilt omgaan met de technische complexiteit van stream processing systemen verwijdert. Gebruik van de [Stream Analytics query taal](https://msdn.microsoft.com/library/azure/dn834998.aspx) in de browser query-editor, krijgt u intelli komen automatisch aanvullen kunt u kunt snel en eenvoudig implementeren vragen over tijd reeks, inclusief tijdelijke gebaseerde joins, een venster aggregaties tijdelijke filters en andere algemene bewerkingen zoals joins, aggregaties prognoses en filters. Bovendien kan browser query testen ten opzichte van een voorbeeld van gegevensbestand snelle, iteratieve ontwikkeling.  

-   **Schaalbaarheid:** Stream Analytics is kan hoog gebeurtenis doorvoer van maximaal 1GB/tweede verwerken. Integratie met [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) en [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) toestaan de oplossing voor het nemen van miljoenen gebeurtenissen per tweede afkomstig zijn van verbonden apparaten, clickstreams, en logboekbestanden, om een paar te noemen. Stream Analytics maakt gebruik van de partities videomogelijkheden van gebeurtenis Hubs, die kan worden verkregen 1 MB/s per partition daartoe. Gebruikers kunnen de berekening in een aantal logische stappen binnen de querydefinitie partitioneren, elk met de mogelijkheid om te worden verdere partitioneren om de schaalbaarheid te verbeteren.  

-   **Betrouwbaarheid, herhaalbaarheid en snel herstel:** Een beheerde service in de cloud, Stream Analytics helpt gegevensverlies voorkomen en bedrijfscontinuïteit in het geval van fouten met behulp van ingebouwde herstel functionaliteit biedt. De service biedt de mogelijkheid intern om status te behouden, herhaald resultaten ervoor zorgen dat is het mogelijk te archiveren gebeurtenissen en opnieuw toepassen verwerken in de toekomst, altijd aan dezelfde resultaten. Hiermee kan klanten gaat u terug in tijd en onderzoeken berekeningen bij het uitvoeren van de onderliggende oorzaak analyseren, wat-als-analyse, enzovoort.  

-   **Lage kosten:** Als een cloudservice, is Stream Analytics geoptimaliseerd voor de gebruikers een zeer lage prijs om aan de slag en onderhouden van realtime analytics-oplossingen bieden. De service is geoptimaliseerd voor u te betalen terwijl u op basis van het gebruik van de eenheid Streaming en de hoeveelheid gegevens die worden verwerkt door het systeem. Gebruik wordt afgeleid op basis van het aantal gebeurtenissen verwerkt en het bedrag van rekenkracht deze is ingericht binnen het cluster u omgaat met de desbetreffende Stream Analytics-taken.  

-   **Verwijzen naar gegevens:** Stream Analytics biedt gebruikers de mogelijkheid kunt opgeven en uw referentiegegevens gebruiken. Dit kan zijn historische of gewoon niet-streaming gegevens die minder vaak na verloop van tijd verandert. Het systeem vergemakkelijkt het gebruik van referentiegegevens worden behandeld als andere binnenkomende gebeurtenis stream deel te nemen aan met andere gebeurtenis streams ingenomen realtime om uit te voeren transformaties.  

-   **Door gebruiker gedefinieerde functies:** Stream Analytics heeft integratie met Azure Machine Learning functie oproepen definiëren als onderdeel van een query Stream analyses van de Machine Learning-service. Hiermee wordt uitgebreid met de mogelijkheden van de Stream Analytics om te profiteren van bestaande Azure Machine Learning-oplossingen. Raadpleeg de [Machine Learning-integratie zelfstudie](stream-analytics-machine-learning-integration-tutorial.md)voor meer informatie hierover.

-   **Connectivity:** Stream Analytics maakt verbinding rechtstreeks naar Azure gebeurtenis Hubs en Azure IoT Hubs voor stream opname en de service Azure Blob om op te nemen historische gegevens. Resultaten kunnen van Stream analyseprogramma naar Azure opslag BLOB's of tabellen, Azure SQL-DB Azure gegevens Lake winkels, DocumentDB, gebeurtenis Hubs, Azure Service Bus onderwerpen of wachtrijen en Power BI, waar kan vervolgens worden weergegeven, verder verwerkt door werkstromen, gebruikt in de batch analytics via [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) of opnieuw verwerkt als een reeks gebeurtenissen worden geschreven. Bij gebruik van de gebeurtenis Hubs is het mogelijk om op te stellen van meerdere Stream Analytics samen met andere gegevensbronnen en verwerking engines zonder de streaming aard van de berekeningen kwijt te raken.  

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
U hebt nu enkele Stream Analytics, een beheerde service voor streaming analytics op gegevens uit de Internet van zaken aan bod. Meer informatie over deze service, raadpleegt u:

- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)

