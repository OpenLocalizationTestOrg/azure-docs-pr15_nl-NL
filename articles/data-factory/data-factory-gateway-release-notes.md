<properties 
    pageTitle="Releaseopmerkingen voor Data Management Gateway | Azure gegevens Factory" 
    description="Data Management Gateway-loop releaseopmerkingen" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Releaseopmerkingen voor Data Management Gateway
Een van de uitdagingen voor moderne gegevensintegratie is naadloos om gegevens te verplaatsen naar en vanuit on-premises naar cloud. Gegevens Factory kunt u deze integratie naadloze met Data Management Gateway, dat wil een agent zeggen of on-premises implementatie om in te schakelen verplaatsing van hybride gegevens kan worden geïnstalleerd.

Zie de volgende artikelen voor meer informatie over Data Management Gateway en hoe u deze gebruiken: 

- [Data Management Gateway](data-factory-data-management-gateway.md)
- [Gegevens verplaatsen tussen de on-premises implementatie en cloud Azure gegevens Factory gebruiken](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Huidige versie (2.2.6072.1)

- Ondersteunt het instellen van HTTP-proxy voor de gateway met de Gateway Configuration Manager. Als geconfigureerd, zijn Azure Blob, Azure tabel Lake van Azure-gegevens en Document DB toegankelijk via het HTTP-proxy.
- Ondersteunt koptekst verwerking voor tekstopmaak als u gegevens van/naar Azure Blob, Azure Lake gegevensopslag, on-premises implementatie bestandssysteem, en on-premises HDFS.
- Ondersteunt het kopiëren van gegevens uit Blob toevoegen en pagina-Blob samen met de al ondersteunde blok Blob.
- Maakt u kennis met een nieuwe status van gateway **Online (beperkt)**, waarmee wordt aangegeven dat de belangrijkste functionaliteit van de gateway, behalve de interactieve bewerking-ondersteuning voor de Wizard kopiëren werkt.
- Verbetert de stabiliteit van gateway registratie registratiegegevens sleutel gebruiken.

## <a name="earlier-versions"></a>Eerdere versies

## <a name="2160401"></a>2.1.6040.1

- Stuurprogramma voor DB2 deel uitmaakt van de gateway-installatiepakket nu. U hoeft niet afzonderlijk te installeren. 
- Stuurprogramma voor DB2 ondersteunt nu z/OS en DB2 voor ik (AS / 400) samen met de platforms al ondersteund (Linux, Unix en Windows). 
- Ondersteunt DocumentDB gebruiken als bron of doel voor on-premises implementatie gegevensopslag
- Ondersteunt het kopiëren van gegevens van/naar verkoudheid/direct blob storage samen met het account al ondersteunde algemene opslag. 
- U kunt verbinding maken met een lokale SQL Server via een gateway met bevoegdheden voor externe aanmelding.  

## <a name="2060131"></a>2.0.6013.1

- U kunt de taal-cultuur moet worden gebruikt door een gateway tijdens de installatie van de handmatige selecteren.
- Als gateway niet werkt zoals verwacht, kunt u de logboeken aan de gateway van de afgelopen zeven dagen naar Microsoft te verzenden om oplossen van problemen met het probleem te vereenvoudigen. Als gateway niet met de cloudservice verbonden is, kunt u kiezen op te slaan en gateway logboeken archiveren.  
- Verbeteringen in de gebruikersinterface voor gateway configuratiemanager:
    - Status van gateway beter zichtbaar maken op het tabblad Start.
    - Opnieuw ingedeelde en vereenvoudigde besturingselementen.
- U kunt gegevens uit een opslagruimte met de [voorbeeldfunctie kopie code-vrije](data-factory-copy-data-wizard-tutorial.md)kopiëren. Zie [Gefaseerde kopiëren](data-factory-copy-activity-performance.md#staged-copy) in het algemeen voor meer informatie over deze functie. 
- U kunt de Data Management Gateway naar ingress gegevens rechtstreeks uit een lokale SQL Server-database naar Azure Machine Learning.
- Prestatieverbeteringen
    - De prestaties verbeteren onder Schema/Preview ten opzichte van SQL Server weergeven in een voorbeeldfunctie kopie code-vrij te geven.



## <a name="11259531"></a>1.12.5953.1
- Correcties

## <a name="11159181"></a>1.11.5918.1

- Maximale grootte van het gebeurtenissenlogboek van de gateway is verhoogd van 1 MB aan 40 MB.
- Een waarschuwing wordt weergegeven in het geval een herstart nodig is tijdens gateway automatisch bijwerken. U kunt kiezen om rechts vervolgens of later opnieuw te starten. 
- Geval automatisch bijwerken mislukt, wordt het installatieprogramma van de gateway pogingen automatische updates driemaal bij maximum.
- Prestatieverbeteringen
    - De prestaties voor het laden van grote tabellen van de server on-premises implementatie in code-vrije kopie scenario verbeteren.
- Correcties

## <a name="11058921"></a>1.10.5892.1

- Prestatieverbeteringen
- Correcties

## <a name="1958652"></a>1.9.5865.2

- De mogelijkheid van nul touch automatisch bijwerken
- Nieuwe taakbalkpictogram met gateway-statusindicatoren
- Mogelijkheid om te "Nu bijwerken" van de client
- Mogelijkheid update planning tijd in te stellen
- PowerShell-script om te schakelen tussen automatisch bijwerken aan/uit
- Ondersteuning voor de indeling van JSON  
- Prestatieverbeteringen
- Correcties

## <a name="1858221"></a>1.8.5822.1

- Probleemoplossing verbeteren
- Prestatieverbeteringen
- Correcties

### <a name="1757951"></a>1.7.5795.1

- Prestatieverbeteringen
- Correcties

### <a name="1757641"></a>1.7.5764.1

- Prestatieverbeteringen
- Correcties

### <a name="1657351"></a>1.6.5735.1

- Ondersteuning voor on-premises HDFS bron/Sink
- Prestatieverbeteringen
- Correcties

### <a name="1656961"></a>1.6.5696.1

- Prestatieverbeteringen
- Correcties

### <a name="1656761"></a>1.6.5676.1

- Diagnostische hulpprogramma's ondersteuning op Configuration Manager
- Tabelkolommen ondersteuning voor gegevensbronnen in tabelvorm voor Factory van Azure-gegevens
- SQL-DW ondersteuning voor Azure gegevens Factory
- Ondersteuning voor Reclusive in BlobSource en FileSource voor Azure gegevens Factory
- Ondersteuning voor CopyBehavior – MergeFiles, PreserveHierarchy en FlattenHierarchy in BlobSink en FileSink met binaire exemplaar voor Azure gegevens Factory
- Ondersteuning van de kopie activiteit rapporteren van de voortgang voor Factory van Azure-gegevens
- Ondersteuning voor bron Connectivity gegevensvalidatie voor Azure gegevens Factory
- Correcties


### <a name="1656721"></a>1.6.5672.1

- De tabelnaam ondersteuning voor ODBC-gegevensbron voor Factory van Azure-gegevens
- Prestatieverbeteringen
- Correcties

### <a name="1656581"></a>1.6.5658.1

- Voor ondersteuningsbestand vangen voor Azure gegevens Factory
- Ondersteuning voor het bevriezen van hiërarchie in binaire kopie voor Factory van Azure-gegevens
- Kopie activiteit Idempotency ondersteuning voor Azure gegevens Factory
- Correcties

### <a name="1656401"></a>1.6.5640.1

- 3 meer gegevensbronnen ondersteuning voor Azure gegevens Factory (ODBC, OData, HDFS)
- Ondersteuning voor aanhalingsteken in CSV-parser voor Factory van Azure-gegevens
- Compressieondersteuning voor (BZip2)
- Correcties

### <a name="1556121"></a>1.5.5612.1

- Ondersteuning van vijf relationele databases voor Azure gegevens Factory (MySQL, PostgreSQL DB2, Teradata en Sybase)
- Compressieondersteuning voor (Gzip en Deflate)
- Prestatieverbeteringen
- Correcties


### <a name="1455491"></a>1.4.5549.1

- Ondersteuning voor de bron van Oracle gegevens voor Factory van Azure-gegevens toevoegen
- Prestatieverbeteringen
- Correcties

### <a name="1454921"></a>1.4.5492.1

- Geïntegreerd binaire die ondersteuning biedt voor zowel Microsoft Azure gegevens fabriek en Office 365 Power BI-services
- Het proces configuratie UI- en registratiegegevens verfijnen
- Azure Data Factory-Azure Ingress- en Egress ondersteuning voor SQL Server-gegevensbron

### <a name="1253031"></a>1.2.5303.1

-   Time-out oplossen om te ondersteunen meer tijdrovende gegevensbronverbindingen. 
    
### <a name="1155268"></a>1.1.5526.8

- .NET Framework 4.5.1 vereist als een vereiste tijdens de installatie.

### <a name="1051442"></a>1.0.5144.2

- Geen wijzigingen die betrekking hebben op Azure gegevens Factory-scenario's. 
