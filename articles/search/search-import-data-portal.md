<properties
    pageTitle="Gegevens importeren in Azure zoekprogramma's via indexeerfuncties in de Portal Azure | Microsoft Azure | De zoekservice gehoste cloud"
    description="De Azure zoeken Wizard importeren gebruiken in de Portal Azure verkenning gegevens van Azure Blob storage, tabel stroage SQL-Database en SQL Server Azure VMs."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Gegevens importeren in Azure zoekprogramma's via de portal

De Azure portal biedt een wizard **Gegevens importeren** op het dashboard Azure zoeken voor het laden van gegevens in een index. 

  ![Gegevens importeren op de opdrachtbalk][1]

Intern, de wizard configureert en roept een *indexering*automatiseren van verschillende stappen van de indexing proces: 

- Verbinding maken met een externe gegevensbron in de huidige Azure abonnement
- Automatisch genereren een schema index op basis van de bron-gegevensstructuur
- Documenten op basis van een rijenset opgehaald uit de gegevensbron maken
- Documenten uploaden naar de index in uw search-service

U kunt deze werkstroom met voorbeeldgegevens in DocumentDB uitproberen. Ga naar [aan de slag met Azure zoeken in de Portal Azure](search-get-started-portal.md) voor instructies.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Gegevensbronnen die worden ondersteund door de Wizard importeren

De wizard importeren ondersteunt de volgende gegevensbronnen: 

- Azure SQL-Database
- Relationele SQL Server-gegevens op een Azure-VM
- Azure DocumentDB
- Azure-blobopslag (in preview)
- Azure-tabelopslag (in preview)

Een afgeplatte gegevensset is een vereiste invoer. U kunt alleen importeren uit één tabel, deelvenster databaseweergave of equivalente gegevensstructuur. U moet deze gegevensstructuur maken voordat u de wizard.

Houd er rekening mee dat een paar van de indexeerfuncties zijn nog steeds in voorbeeld, wat betekent dat de indexering definitie wordt ondersteund door de preview-versie van de API. Zie [overzicht van de indexering](search-indexer-overview.md) voor meer informatie en koppelingen.

## <a name="connect-to-your-data"></a>Verbinding maken met uw gegevens

1. Meld u aan bij de [Portal van de Azure](https://portal.azure.com) en open servicedashboard. U kunt **zoekservices** klikken in de balk gaat u naar de bestaande services weergeven in het huidige abonnement. 

2. Klik op **Gegevens importeren** op de opdrachtbalk naar dia openen, het blad gegevens importeren.  

3. Klik op **verbinding maken met uw gegevens** om op te geven van een definitie van de gegevensbron die wordt gebruikt door een indexering. Gegevensbronnen binnen-abonnement, kunt de wizard meestal detecteren en Lees voor informatie over de databaseverbinding, totale configuratie vereiste minimaliseren.

| | |
|--------|------------|
|**Bestaande gegevensbron** | Als u al indexeerfuncties gedefinieerd in de zoekservice hebt, kunt u een bestaande definitie van de gegevensbron voor een andere importeren.|
|**Azure SQL-Database** | De naam van de service, referenties voor de databasegebruiker van een met leesmachtiging en naam van een database kan worden opgegeven op de pagina of via een ADO.NET-verbindingsreeks. Kies de optie verbinding tekenreeks om te bekijken of eigenschappen aanpassen. <br/><br/>De tabel of weergave waarmee de rijenset moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding is gemaakt, geeft een vervolgkeuzelijst zodat u kunt een selectie maken.|
|**SQL Server Azure VM** | Geef een volledige servicenaam, gebruikers-ID en wachtwoord en database als een verbindingsreeks. Deze als gegevensbron wilt gebruiken, moet u hebt eerder geïnstalleerd een certificaat in het lokale archief waarbij worden gecodeerd voor de verbinding. <br/><br/>De tabel of weergave waarmee de rijenset moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding is gemaakt, geeft een vervolgkeuzelijst zodat u kunt een selectie maken.
|**DocumentDB** |Vereisten kunnen u het account, database en siteverzamelingen. Alle documenten in de siteverzameling worden opgenomen in de index. U kunt een query wilt samenvoegen of de rijenset filteren of om te bepalen de gewijzigde documenten voor de volgende gegevens vernieuwingsbewerkingen definiëren.|
|**Azure-blobopslag** | Vereisten voor opnemen de opslag-account en een container. (Optioneel) als blob namen een virtuele naamgevingsconventie voor groepering doeleinden volgt, kunt u het gedeelte virtuele map van de naam als een map onder container. Zie [Indexeren Blob Storage (preview)](search-howto-indexing-azure-blob-storage.md) voor meer informatie. |
|**Azure-tabelopslag** | Vereisten voor opnemen de opslag-account en de naam van een tabel. Desgewenst kunt u een query voor het ophalen van een subset van de tabellen. Zie [Indexeren Table Storage (preview)](search-howto-indexing-azure-tables.md) voor meer informatie. |

## <a name="customize-target-index"></a>Target index aanpassen

Als u een voorlopige index is meestal afgeleid van de gegevensset. Toevoegen, bewerken of verwijderen van velden om te voltooien het schema. Bovendien kenmerken instellen op het veldniveau van het om te bepalen de gedrag van de volgende zoeken.

1. Geef in de **doel-index aanpassen**, de naam en een **toets** gebruikt om elk document uniek te identificeren. De sleutel moet een tekenreeks. Als veldwaarden geen spaties of streepjes Zorg ervoor dat u geavanceerde opties instellen in **uw gegevens importeren** in de validatie controleren op deze tekens bevat onderdrukken.

2. Bekijken en bewerken van de resterende velden. Veldnaam en type zijn meestal ingevuld voor u. U kunt het gegevenstype wijzigen.

3. Indexkenmerken voor elk veld instellen:

 - Opvraagbare is, retourneert het veld in de lijst met zoekresultaten.
 - Filterbare kunt u het veld waarnaar u wilt verwijzen in filterexpressies.
 - Sorteerbaar kunt u het veld moet worden gebruikt in een sorteren.
 - Facetable kunt het veld voor beperkte navigatie.
 - Doorzoekbare kunt zoeken in volledige tekst.
  
4. Klik op het tabblad **Analyzer** als u wilt een analyzer taal op het veldniveau van het opgeven. Alleen taal analyzers kunnen op dit moment worden opgegeven. Met een aangepaste analyzer of een niet-taal analyzer zoals trefwoord, patroon en dergelijke, vereist code.

 - Klik op **Searchable** om te zoeken in volledige tekst op het veld aanwijzen en inschakelen van de vervolgkeuzelijst Analyzer.
 - Kies de gewenste analyzer. Zie [maken een index voor documenten in meerdere taal](search-language-support.md) voor meer informatie.

5. Klik op de **Suggester** om in te schakelen voor tekst aanvullen Querysuggesties op de geselecteerde velden.


## <a name="import-your-data"></a>Uw gegevens importeren

1. Geef een naam voor de indexering in **uw gegevens importeren**. Intrekken dat het product van de wizard importeren een indexering is. Later, als u wilt bekijken of bewerken, kiest u deze in de portal niet op basis van de wizard opnieuw. 

2. Geef op de planning, die is gebaseerd op de regionale tijdzone waarin de service is ingericht.

3. Geavanceerde opties instellen voor het opgeven van drempelwaarden op of de indexering blijven kan als een document wordt verwijderd. Bovendien kunt u opgeven of **toets** velden mogen spaties en streepjes.  

## <a name="edit-an-existing-indexer"></a>Een bestaande indexering bewerken

Dubbelklik op de tegel indexering voor een lijst met alle indexeerfuncties gemaakt voor uw abonnement in het servicedashboard. Dubbelklik op een van de indexeerfuncties wilt uitvoeren, bewerken of verwijderen. U kunt de index vervangen door een andere bestaande, wijzigen van de gegevensbron en opties instellen voor drempelwaarden fout tijdens het indexeren.

## <a name="edit-an-existing-index"></a>Een bestaande index bewerken

In Azure zoeken, structurele updates voor een index moeten worden opgebouwd die index, die bestaat uit de index verwijderen, opnieuw de index te maken en gegevens opnieuw te laden. Structurele updates bestaan wijzigen een gegevenstype en de naam wijzigen of verwijderen van een veld.

Bewerkingen waarvoor niet nodig voor een opnieuw opbouwen is omvatten het toevoegen van een nieuw veld, score profielen suggesters wijzigen, of het wijzigen van taal analyzers wijzigen. Zie [Index bijwerken](https://msdn.microsoft.com/library/azure/dn800964.aspx) voor meer informatie.

## <a name="next-step"></a>Volgende stap

Bekijk deze koppelingen voor meer informatie over indexeerfuncties:

- [Indexering Azure SQL-Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB indexeren](../documentdb/documentdb-search-indexer.md)
- [Indexering Blob Storage (preview)](search-howto-indexing-azure-blob-storage.md)
- [Indexering Table Storage (preview)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

