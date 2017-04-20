<properties
    pageTitle="Gegevens importeren in Machine Learning Studio uit online gegevensbronnen | Microsoft Azure"
    description="Het importeren van uw trainingsgegevens Azure Machine Learning Studio uit verschillende onlinebronnen."
    keywords="gegevensindeling, gegevenstypen, gegevensbronnen, trainingsgegevens importeren"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Gegevens importeren in Azure Machine Learning Studio uit verschillende bronnen van online-gegevens met de module gegevens importeren

In dit artikel worden de ondersteuning voor online-gegevens uit verschillende bronnen en de informatie die nodig is om gegevens te verplaatsen uit deze bronnen in een experiment Azure Machine Learning importeren.

> [AZURE.NOTE] In dit artikel bevat algemene informatie over de [Gegevens importeren] [ import-data] module. Voor meer informatie over de verschillende typen gegevens die u openen gedetailleerde kunt, opmaak, parameters en antwoorden op veelgestelde vragen, raadpleegt u het onderwerp van de module naslaginformatie voor de [Gegevens importeren] [ import-data] module.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Inleiding

Kunt u gegevens uit de Azure Machine Learning Studio openen vanaf een van de verschillende online gegevensbronnen terwijl uw experiment met behulp van de [Gegevens importeren] wordt uitgevoerd[ import-data] module:

- Een Web-URL met behulp van HTTP
- Hadoop HiveQL gebruiken
- Azure-blobopslag
- Azure tabel
- Azure SQL-database of SQL Server Azure VM
- Lokale SQL Server-database
- Een provider, momenteel OData-gegevensfeed
 
De werkstroom voor het verrichten experimenten in Azure Machine Learning Studio bestaat uit slepen en neerzetten-onderdelen op het tekenpapier. [Gegevens importeren] toevoegen voor toegang tot online gegevensbronnen,[ import-data] module naar uw experiment, selecteert u de **gegevensbron**en geef de parameters die nodig zijn voor toegang tot de gegevens. De online gegevensbronnen die worden ondersteund worden in de onderstaande tabel gespecificeerde. Deze tabel worden ook de bestandsindelingen die worden ondersteund en parameters die worden gebruikt voor toegang tot de gegevens.

Houd er rekening mee dat omdat deze trainingsgegevens wordt geopend terwijl uw experiment wordt uitgevoerd, dit alleen beschikbaar in die experiment is. Vergelijking: de gegevens die is opgeslagen in een module gegevensset zijn beschikbaar voor een experiment in uw werkruimte.

> [AZURE.IMPORTANT] Op dit moment de [Gegevens importeren] [ import-data] en [Gegevens exporteren] [ export-data] modules kunnen lezen en schrijven gegevens alleen uit Azure opslag die zijn gemaakt met behulp van het model Klassiek implementatie. Met andere woorden, is de nieuwe Azure-blobopslag accounttype die een warm opslag access laag of leuke opslag access laag biedt nog niet ondersteund. 

> In het algemeen wordt uw Azure opslag-accounts dat u mogelijk hebt gemaakt voordat u deze serviceoptie beschikbaar kwam moet niet worden beïnvloed. Als u een nieuw account maken moet, **klassieke** selecteren voor het model implementatie, of resourcemanager gebruiken en voor **type Account**, selecteert u **algemene** in plaats van **blobopslag**. 

> Zie voor meer informatie [Azure-blobopslag: warm- en leuke opslag lagen](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Ondersteunde online gegevensbronnen
Azure Machine Learning- **Gegevens importeren** module ondersteunt de volgende gegevensbronnen:

Gegevensbron | Beschrijving | Parameters |
---|---|---|
Web-URL via HTTP |Leest gegevens in door komma's gescheiden waarden (CSV), door tabs gescheiden waarden (), kenmerk-relatie-bestandsindeling (ARFF) en indelingen voor ondersteuning Vector Machines (SVM-licht), van een web-URL die wordt gebruikt HTTP | <b>URL</b>: Hiermee geeft u de volledige naam van het bestand, inclusief de URL van de site en de bestandsnaam, met willekeurige toestellen. <br/><br/><b>Gegevensindeling</b>: Hiermee geeft u een van de ondersteunde gegevens indelingen: CSV-, TSV, ARFF of SVM-light. Als de gegevens een veldnamenrij bevat, wordt deze gebruikt voor het toewijzen van kolomnamen.|
Hadoop/HDFS|Leest gegevens van gedistribueerde opslagruimte in Hadoop. U opgeven de gegevens die u met behulp van HiveQL, een querytaal voor SQL-achtige wilt. HiveQL kunnen ook worden gebruikt om te aggregeren, gegevens en daar voordat u de gegevens aan Machine Learning Studio toevoegen filteren van gegevens.|<b>Component databasequery's</b>: Hiermee geeft u de component query gebruikt om de gegevens te genereren.<br/><br/><b>HCatalog server-URI</b> : de naam van uw cluster met de opmaak *opgegeven &lt;de clusternaam van uw&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop gebruikersnaam</b>: Hiermee geeft u de Hadoop-gebruikersaccount gebruiken dat is gebruikt voor het inrichten van het cluster.<br/><br/><b>Wachtwoord voor een gebruikersaccount Hadoop</b> : Hiermee geeft u de referenties die bij de inrichting van het cluster gebruikt. Zie [maken Hadoop clusters in HDInsight](../hdinsight/hdinsight-provision-clusters.md)voor meer informatie.<br/><br/><b>Locatie van uitvoergegevens</b>: Hiermee wordt opgegeven of de gegevens worden opgeslagen in Azure wordt aangegeven of in een Hadoop distributed bestand system (HDFS). <br/><ul>Als u uitvoergegevens in HDFS worden opgeslagen, geeft u de server HDFS URI. (Zorg ervoor dat de naam van het HDInsight cluster zonder het voorvoegsel HTTPS:// gebruiken). <br/><br/>Als u uw uitvoergegevens in Azure wordt aangegeven opslaat, moet u de Azure-opslagaccountnaam, de opslag toegangstoets en de naam van de container gegevensopslag.</ul>|
SQL-database |Gegevens die zijn opgeslagen in een Azure SQL-database of in een SQL Server-database op een Azure virtuele machines uitgevoerd leest. | <b>Naam van de database-server</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><ul>Voer de naam van de server die wordt gegenereerd voor het geval Azure SQL-Database. Meestal heeft het formulier * &lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Voer in het geval van een SQL server die worden gehost op een Azure Virtual machine *tcp:&lt;VM DNS-naam&gt;, 1433*</ul><br/><b>Databasenaam </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>Server-account gebruikersnaam</b>: Hiermee geeft u een gebruikersnaam in te voeren voor een account met machtigingen voor de database. <br/><br/><b>Wachtwoord voor een gebruikersaccount server</b>: Hiermee geeft u het wachtwoord voor het gebruikersaccount.<br/><br/><b>Een servercertificaat geaccepteerd</b>: Gebruik deze optie (minder veilig) als u overslaan reviseren van het site-certificaat wilt voordat u uw gegevens lezen.<br/><br/><b>Databasequery's</b>: Voer een SQL-instructie waarmee de gegevens die u wilt lezen wordt beschreven.|
On-premises implementatie SQL-database |Leest gegevens die zijn opgeslagen in een lokale SQL-database. | <b>Gateway van gegevens</b>: de naam van de Data Management Gateway is geïnstalleerd op een computer waarop deze toegang uw SQL Server-database tot hebben. Zie voor informatie over het instellen van de gateway [uitvoeren geavanceerde analyses met Azure Machine Learning gebruikmaken van gegevens uit een lokale SQL server](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Naam van de database-server</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><br/><b>Databasenaam </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>Server-account gebruikersnaam</b>: Hiermee geeft u een gebruikersnaam in te voeren voor een account met machtigingen voor de database. <br/><br/><b>Gebruikersnaam en wachtwoord</b>: klikt u op <b>Enter waarden</b> om uw database. U kunt Windows-verificatie of SQL Server-verificatie is afhankelijk van hoe uw on-premises implementatie SQL Server is geconfigureerd.<br/><br/><b>Databasequery's</b>: Voer een SQL-instructie waarmee de gegevens die u wilt lezen wordt beschreven.|
Azure tabel|Leest gegevens van de tabel-service in Azure Storage.<br/><br/>Als u grote hoeveelheden gegevens niet vaak worden gelezen, gebruikt u de tabel Azure-Service. Deze biedt een flexibele, niet-relationele (NoSQL), sterk schaalbare, goedkoop en maximaal beschikbare opslagruimte oplossing.| De opties in de **Gegevens importeren** afwijken, afhankelijk van of u toegang krijgt tot openbare informatie of een account, persoonlijke opslag waarmee aanmeldingsreferenties vereist. Hiermee wordt bepaald door het <b>Verificatietype</b> waarvoor de waarde van "PublicOrSAS" of 'Account', die elk een eigen set met parameters heeft. <br/><br/><b>Openbare of gedeelde Access handtekening (SA's) URI</b>: de parameters zijn:<br/><br/><ul><b>Tabel-URI</b>: Hiermee geeft u het publiek of de URL van de SA's voor de tabel.<br/><br/><b>Hiermee geeft u de rijen om te zoeken naar eigenschapnamen</b>: de waarden zijn <i>TopN</i> aan het scannen van het opgegeven aantal rijen of <i>ScanAll</i> om alle rijen in de tabel. <br/><br/>Als de gegevens zich homogene en overzichtelijk, is het raadzaam dat u selecteert u *TopN* en typt u een waarde voor N. Voor grote tabellen, kan dit resulteren in sneller lezen tijden.<br/><br/>Als de gegevens is gestructureerd met sets eigenschappen variëren op basis van de diepteas en de positie van de tabel, kiest u de optie *ScanAll* voor het scannen van alle rijen. Dit zorgt ervoor dat de integriteit van de resulterende eigenschap en metagegevens conversie.<br/><br/></ul><b>Persoonlijke opslag Account</b>: de parameters zijn: <br/><br/><ul><b>Accountnaam</b>: Hiermee geeft u de naam van het account dat de tabel te lezen bevat.<br/><br/><b>Accountsleutel</b>: Hiermee geeft u de opslag-toets die is gekoppeld aan het account.<br/><br/><b>Tabelnaam</b> : Hiermee geeft u de naam van de tabel met de gegevens te lezen.<br/><br/><b>Rijen om te zoeken naar eigenschapnamen</b>: de waarden zijn <i>TopN</i> aan het scannen van het opgegeven aantal rijen of <i>ScanAll</i> om alle rijen in de tabel.<br/><br/>Als de gegevens zich homogene en overzichtelijk, raden die u selecteert u *TopN* en voer een van de N. Voor grote tabellen, kan dit resulteren in sneller lezen tijden.<br/><br/>Als de gegevens is gestructureerd met sets eigenschappen variëren op basis van de diepteas en de positie van de tabel, kiest u de optie *ScanAll* voor het scannen van alle rijen. Dit zorgt ervoor dat de integriteit van de resulterende eigenschap en metagegevens conversie.<br/><br/>|
Azure-blobopslag | Gegevens die zijn opgeslagen in de service Blob in Azure-opslag, zoals afbeeldingen, ongestructureerde tekst of binaire gegevens leest.<br/><br/>U kunt de service Blob gegevens openbaar toegankelijk te maken of privé om toepassingsgegevens te slaan. U kunt toegang tot uw gegevens vanaf elke locatie met behulp van HTTP of HTTPS-verbindingen. | De opties in de module **Importgegevens** afwijken, afhankelijk van of u toegang krijgt tot openbare informatie of een account, persoonlijke opslag waarmee aanmeldingsreferenties vereist. Hiermee wordt bepaald door het <b>Verificatietype</b> die een waarde van "PublicOrSAS" of 'Account' kan bevatten.<br/><br/><b>Openbare of gedeelde Access handtekening (SA's) URI</b>: de parameters zijn:<br/><br/><ul><b>URI</b>: de openbare of SA's URL opgeeft voor de blob storage.<br/><br/><b>Bestandsindeling</b>: Hiermee geeft u de opmaak van de gegevens in de Blob-service. De ondersteunde indelingen zijn CSV-, TSV en ARFF.<br/><br/></ul><b>Persoonlijke opslag Account</b>: de parameters zijn: <br/><br/><ul><b>Accountnaam</b>: Hiermee geeft u de naam van het account dat de blob die u wilt lezen bevat.<br/><br/><b>Accountsleutel</b>: Hiermee geeft u de opslag-toets die is gekoppeld aan het account.<br/><br/><b>Pad naar container, map, of blob</b> : Hiermee geeft u de naam van de blob met de gegevens te lezen.<br/><br/><b>BLOB-bestandsindeling</b>: Hiermee geeft u de opmaak van de gegevens in de blob-service. De ondersteunde gegevensindelingen zijn CSV-, TSV, ARFF, CSV met een opgegeven codering en Excel. <br/><br/><ul>Als de notatie CSV- of TSV, zorg ervoor dat u om aan te geven of het bestand een veldnamenrij bevat.<br/><br/>U kunt de optie Excel gebruiken om te lezen van gegevens uit Excel-werkmappen. In de <i>bestandsindeling van Excel-gegevens</i> , aangeven of de gegevens in een Excel-werkbladbereik of in een Excel-tabel is. Geef in de optie <i>Excel-werkblad of de ingesloten tabel </i>, de naam van het blad of de tabel die u wilt lezen.</ul><br/>|
Gegevensfeedprovider | Leest gegevens van een ondersteunde feeds provider. Momenteel alleen in de Open Data Protocol (OData)-indeling wordt ondersteund. | <b>Inhoudstype van gegevens</b>: Hiermee wordt de OData-indeling.<br/><br/><b>Bron-URL</b>: Hiermee geeft u de volledige URL voor de gegevensfeed. <br/>Bijvoorbeeld de volgende URL van de voorbeelddatabase Noordenwind leest: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
