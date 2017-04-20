<properties
   pageTitle="Uw gegevens migreren naar SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het migreren van uw gegevens naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Migreren van uw gegevens
Gegevens kunt uit verschillende bronnen in uw SQL Data Warehouse met een diverse hulpprogramma's worden verplaatst.  ADF kopiëren, SSIS en bcp kunnen alle te realiseren worden gebruikt. Echter als de hoeveelheid gegevens verhoogt moet u nadenken het migratieproces in stappen splitsen. Hierdoor kunnen u de mogelijkheid om het optimaliseren van elke stap voor de prestaties zowel voor flexibiliteit voor een gegevensmigratie vloeiende zorgen.

Dit artikel worden eerst de eenvoudige migratie scenario's van ADF kopiëren, SSIS en bcp. Deze kijkt u iets grondigere in hoe de migratie kan worden geoptimaliseerd.

## <a name="azure-data-factory-adf-copy"></a>Azure gegevens Factory (ADF) kopiëren
[ADF kopie][] maakt deel uit van de [Azure gegevens fabriek][]. Uw gegevens exporteren naar een platte bestanden op de lokale opslag, tot externe platte bestanden gehouden in Azure-blobopslag of rechtstreeks in SQL Data Warehouse kunt u ADF kopiëren.

Als uw gegevens wordt gestart in platte bestanden en klikt u eerst overbrengen naar opslag Azure blob moet voordat u begint een laden in SQL Data Warehouse. Nadat de gegevens worden overgebracht naar Azure-blobopslag kunt u de [Kopie van de ADF][] opnieuw gebruiken om de gegevens in SQL Data Warehouse.

PolyBase bevat ook een high-performance-optie voor het laden van de gegevens. Dat betekent echter met twee hulpmiddelen in plaats van één. Als u moet de beste prestaties PolyBase gebruiken. Als u een ervaring één hulpmiddel (en de gegevens die zich niet enorme) is ADF uw antwoord.

> [AZURE.NOTE] PolyBase is vereist voor de gegevensbestanden in UTF-8. Dit is de ADF kopie standaardwaarde codering zodat er geen te wijzigen. Dit is alleen een herinnering het standaardgedrag van ADF kopie niet wijzigen.

Onderweg via naar het volgende artikel voor enkele uitstekende [ADF voorbeelden][].

## <a name="integration-services"></a>Services voor adreslijstintegratie ##
Integration Services (SSIS) is een krachtige en flexibele extraheren transformeren en laden (ETL) hulpprogramma die ondersteuning biedt voor complexe werkstromen, gegevenstransformatie en verschillende opties voor het laden van gegevens. Gebruik SSIS om gegevens over te dragen aan Azure of als onderdeel van een uitgebreidere migratie.

> [AZURE.NOTE] SSIS kunt exporteren naar UTF-8 zonder de markering byte volgorde in het bestand. Als u wilt configureren Gebruik dit moet u eerst het onderdeel afgeleide kolom om de tekengegevens in de gegevensstroom gebruik van de 65001 UTF-8-codetabel te converteren. Zodra de kolommen zijn geconverteerd, kunt u de gegevens schrijven naar de plat bestand bestemming adapter ervoor te zorgen dat 65001 ook als de pagina voor het bestand is geselecteerd.

SSIS maakt verbinding met SQL Data Warehouse net zoals u zou u verbinding met een SQL Server-implementatie. Uw verbindingen moet echter gebruikmaken van een manager ADO.NET-verbinding. U moet ook handelingen kunt verrichten voor het configureren van de 'Gebruik bulksgewijs invoegen indien beschikbaar' instelling doorvoer maximaliseren. Raadpleeg het artikel [ADO.NET bestemming adapter][] voor meer informatie over deze eigenschap

> [AZURE.NOTE] Verbinding maken met Azure SQL Data Warehouse met OLEDB wordt niet ondersteund.

Daarnaast is er altijd de mogelijkheid dat een pakket worden mogelijk niet einddatum beperken-of netwerkproblemen. Ontwerp pakketten zodat u ze kunnen hervatten op het moment is mislukt, zonder dat voltooid voordat de fout opnieuw uitvoeren werken.

Raadpleeg de [SSIS-documentatie][]voor meer informatie.

## <a name="bcp"></a>BCP
BCP is een hulpprogramma voor de opdrachtregel die is bedoeld voor plat bestand gegevens importeren / exporteren. Sommige transformatie kan plaatsvinden tijdens het exporteren van gegevens. Als u wilt uitvoeren eenvoudige transformaties een query gebruiken om te selecteren en de gegevens transformeren. Nadat geëxporteerd, kunnen de platte bestanden vervolgens worden geladen rechtstreeks in het doel de SQL Data Warehouse-database.

> [AZURE.NOTE] Dit is het meestal een goed idee om de transformaties tijdens het exporteren van gegevens in een weergave op het brone-mailsysteem gebruikt onderbrengen. Dit zorgt ervoor dat de logica behouden blijft en het proces herhaald wordt.

Voordelen van bcp zijn:

- Eenvoudig. BCP-opdrachten zijn eenvoudige maken en uitvoeren
- Meer gestart worden opnieuw laden proces. Eenmaal geëxporteerde het selectievakje laden kan worden uitgevoerd van een willekeurig aantal malen

Beperkingen van bcp zijn:

- BCP werkt met alleen gebruikmaking platte bestanden. Dit werkt niet met bestanden zoals XML- of JSON
- BCP biedt geen ondersteuning voor het exporteren naar UTF-8. Hiermee kan verhinderen dat PolyBase op bcp geëxporteerd gegevens gebruiken
- Mogelijkheden om gegevens transformatie zijn beperkt tot het podium exporteren alleen en eenvoudig van aard zijn
- BCP is niet aangepast aan het robuuste worden bij het laden van gegevens via internet. Instabiliteit netwerk mogelijk een fout bij het laden.
- BCP is afhankelijk van het schema aanwezig zijn in de doeldatabase voordat u het selectievakje laden

Zie voor meer informatie [bcp gebruiken om gegevens in SQL Data Warehouse te laden][].

## <a name="optimizing-data-migration"></a>Optimaliseren van de gegevensmigratie
Een migratieproces SQLDW kan worden effectief opgedeeld in drie aparte stappen:

1. Exporteren van brongegevens
2. Overdracht van gegevens naar Azure
3. In de doeldatabase SQLDW laden

Elke stap kan afzonderlijk worden geoptimaliseerd als u wilt maken van een robuuste, opnieuw meer gestart en robuuste migratieproces die gemaximaliseerd prestaties bij elke stap.

## <a name="optimizing-data-load"></a>Optimaliseren gegevens laden
Zoek op de volgende in omgekeerde volgorde voor even; de snelste manier om gegevens te laden is via PolyBase. Optimaliseren voor een PolyBase laden proces geplaatst vereisten op de voorgaande stappen is het verstandig om te begrijpen dat dit tevoren alles. Ze zijn:

1. Codering van gegevensbestanden
2. Opmaak van gegevensbestanden
3. Locatie van gegevensbestanden

### <a name="encoding"></a>Coderen
PolyBase vereist gegevensbestanden moeten UTF-8-codering. Dit betekent dat wanneer u uw gegevens exporteren deze aan deze vereiste voldoen moet. Als uw gegevens bevat alleen eenvoudige ASCII-tekens (niet extended ASCII) klikt u vervolgens deze map rechtstreeks naar de standaard UTF-8 en u niet hoeft te hoeft te weten de codering. Echter als uw gegevens geen speciale tekens bevatten zoals umlauts, accenten of symbolen of uw gegevens niet-Latijnse talen ondersteunt wordt hebt u om ervoor te zorgen dat uw bestanden exporteren correct UTF-8 zijn-codering.

> [AZURE.NOTE] BCP biedt geen ondersteuning voor het exporteren van gegevens naar UTF-8. De beste optie dus Integration Services of ADF kopiëren gebruiken voor het exporteren van gegevens. Het verdient aanbeveling af te wijzen dat de UTF-8 bytes volgorde markering in het gegevensbestand is vereist.

Gecodeerd met UTF-16 bestanden moeten worden opnieuw geschreven ***voorafgaand*** aan de gegevensoverdracht.

### <a name="format-of-data-files"></a>Opmaak van gegevensbestanden
PolyBase bepaalt een vaste rij-einde van \n of nieuwe regel. De gegevensbestanden moeten voldoen aan deze standaard. Er zijn beperkingen van de tekenreeks of kolom afsluitingen niet.

U moet elke kolom in het bestand als onderdeel van uw externe tabel in PolyBase definiëren. Zorg ervoor dat alle geëxporteerde kolommen vereist zijn en dat de typen aan de vereiste normen voldoen.

Raadpleeg terug naar de [migreren uw schema] artikel voor informatie over ondersteunde gegevenstypen.

### <a name="location-of-data-files"></a>Locatie van gegevensbestanden
PolyBase SQL Data Warehouse gebruikt om gegevens te laden van Azure-blobopslag uitsluitend. Daarom zijn de gegevens eerst overgebracht naar blobopslag.

## <a name="optimizing-data-transfer"></a>Optimaliseren overdracht van gegevens
Een van de laagst mogelijke onderdelen van de gegevensmigratie is de overdracht van de gegevens naar Azure. Niet alleen kan netwerkbandbreedte wel een probleem, maar ook netwerk betrouwbaarheid serieus voortgang kunt nadelig. Migreren van gegevens naar Azure is standaard via internet zodat de kans op doorverbinden-fouten redelijk waarschijnlijk. Deze fouten kunnen echter gegevens moeten opnieuw worden verzonden geheel of gedeeltelijk nodig.

Gelukkig hebt u verschillende opties om de snelheid en flexibiliteit van dit proces te verbeteren:

### <a name="expressroute"></a>[ExpressRoute][]
U kunt u overwegen [ExpressRoute][] versnellen de overdracht. [ExpressRoute][] biedt een privé verbinding naar Azure zodat de verbinding niet verder via de openbare internet. Dit is in geen geval een verplichte stap. Echter deze doorvoer verbeteren wanneer u gegevens naar Azure doordat vanuit een on-premises of collega locatie faciliteit.

De voordelen van het gebruik van [ExpressRoute][] zijn:

1. Verbeterde betrouwbaarheid
2. Sneller netwerk
3. Lagere netwerklatentie
4. hogere netwerkbeveiliging

[ExpressRoute][] is handig voor een aantal scenario's; niet alleen de migratie.

Geïnteresseerd? Ga naar de [ExpressRoute documentatie][]voor meer informatie en neem prijzen.

### <a name="azure-import-and-export-service"></a>Azure importeren en exporteren-Service
De Azure importeren en exporteren-Service is een procedure voor het doorverbinden van gegevens voor grote (GB ++) ontworpen voor enorme (TB ++) overdrachten van gegevens in Azure. Het gaat hierbij om uw gegevens worden geschreven naar schijven en het verzenden naar een Azure Datacenter. De inhoud van de schijf wordt vervolgens in Azure opslag BLOB's namens worden geladen.

Een overzichtsweergave van de resultaten van het importproces exporteren is als volgt:

1. Een container Azure-blobopslag om de gegevens te ontvangen configureren
2. Uw gegevens exporteren naar de lokale opslag
2. Kopieer de gegevens naar 3,5 inch SATA II/III vaste schijven hulpprogramma voor het [Azure importeren/exporteren]
3. Maken van een taak, importeren die gebruikmaakt van de Azure importeren en exporteren Service leveren van de logboek-bestanden die met de [importeren/exporteren hulpprogramma Azure]
4. Uw aangewezen Azure-datacenter van de schijven verzenden
5. Uw gegevens overgebracht naar de container Azure-blobopslag
6. De gegevens laadt in SQLDW PolyBase gebruiken

### <a name="azcopy-utility"></a>[AZCopy][] hulpprogramma
Het hulpprogramma [AZCopy][] is een prima hulpmiddel voor het ophalen van uw gegevens worden overgedragen naar Azure opslag BLOB's. Dit is bedoeld voor kleine (MB ++) naar zeer grote (GB ++) gegevens overdrachten. [AZCopy] heeft ook zijn ontworpen voor goede robuuste doorvoer wanneer gegevens overbrengen naar Azure en dat is een uitstekende keus voor de stap gegevens de overdracht. Eenmaal overgedragen kunt u de gegevens met PolyBase in SQL Data Warehouse laden. U kunt ook AZCopy opnemen in uw SSIS-pakketten met een taak 'Proces uitvoeren'.

Als u wilt gebruiken AZCopy moet u eerst om te downloaden en installeren. Er is een [versie][] en een [preview-versie][] beschikbaar.

Als u wilt uploaden van een bestand vanaf uw bestandssysteem moet u een opdracht zoals hieronder:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Een samenvatting op hoog niveau proces mogelijk:

1. Een container opslag Azure blob om de gegevens te ontvangen configureren
2. Uw gegevens exporteren naar de lokale opslag
3. AZCopy uw gegevens in de container Azure-blobopslag
4. De gegevens laadt in SQL Data Warehouse PolyBase gebruiken

Volledig documentatie beschikbaar: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimaliseren gegevensexport
Naast het ervoor te zorgen dat de export aan de vereisten die is opgemaakt in PolyBase voldoet kunt u ook proberen te optimaliseren van de export van de gegevens naar het proces verder te verbeteren.

> [AZURE.NOTE] PolyBase de gegevens wilt invoegen in UTF-8-indeling waarschijnlijk niet nodig kunt u bcp wilt gebruiken voor het exporteren van gegevens uitvoeren. BCP biedt geen ondersteuning voor het uitvoeren van gegevensbestanden in UTF-8. SSIS of ADF kopiëren zijn veel beter geschikt is voor de uitvoering van dit soort gegevens exporteren.

### <a name="data-compression"></a>Gegevenscompressie
Gzip gecomprimeerd gegevens kunnen worden gelezen door PolyBase. Als u kunt uw gegevens om de gzip-bestanden comprimeren wordt u de hoeveelheid gegevens die via het netwerk wordt gedrukt minimaliseren.

### <a name="multiple-files"></a>Meerdere bestanden
Grote tabellen in verschillende bestanden splitsen niet alleen helpt bij het sneller exporteren, is het ook handig met doorverbinden re-startability en de algehele beheerbaarheid van de gegevens eenmaal in het Azure-blobopslag. Een van de vele nette voorzieningen PolyBase is Hiermee worden alle bestanden in een map lezen en deze behandelen als één tabel. Daarom een goed idee om de bestanden die u voor elke tabel in een aparte map isoleren.

PolyBase ondersteunt ook een functie 'recursieve map transport' genoemd. Deze functie kunt u de organisatie van uw geëxporteerde gegevens op het verbeteren van het beheren van uw gegevens verder te verbeteren.

Zie voor meer informatie over het laden van gegevens met PolyBase, [PolyBase gebruiken om gegevens in SQL Data Warehouse te laden][].


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over migratie, [migreren uw oplossing naar SQL Data Warehouse][].
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[ADF kopiëren]: ../data-factory/data-factory-data-movement-activities.md 
[ADF voorbeelden]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md
[Uw oplossing migreren naar SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Bcp gebruiken om gegevens te laden in SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[PolyBase gebruiken om gegevens te laden in SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure gegevens Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentatie]: http://azure.microsoft.com/documentation/services/expressroute/

[versie van de]: http://aka.ms/downloadazcopy/
[Preview-versie]: http://aka.ms/downloadazcopypr/
[ADO.NET bestemming adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS-documentatie]: https://msdn.microsoft.com/library/ms141026.aspx
