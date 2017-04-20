<properties 
    pageTitle="Gegevens verplaatsen tussen schaal-out cloud databases | Microsoft Azure" 
    description="Wordt uitgelegd hoe manipuleren shards en verplaats gegevens via een selfservice gehoste service elastische database API's gebruikt." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Gegevens verplaatsen tussen schaal-out cloud-databases

Als u een Software als Service ontwikkelaar bent en ineens uw app enorme aanvraag betrokken, moet u zijn voor de groei. Zodat toevoegen u meer databases (shards). Hoe u de gegevens naar de nieuwe databases verspreiden zonder storend voor de integriteit van gegevens? Gebruik het **hulpmiddel gesplitste samenvoegen** om gegevens te verplaatsen uit beperkte databases met de nieuwe databases.  

Het hulpmiddel gesplitste samenvoegen wordt uitgevoerd als een Azure webservice. Een beheerder of ontwikkelaar gebruikt het hulpmiddel shardlets (gegevens uit een shard) verplaatsen tussen verschillende databases (shards). Het hulpmiddel maakt gebruik van shard kaart management voor het behoud van de database van de service-metagegevens en controleer of consistente toewijzingen.

![Overzicht][1]

## <a name="download"></a>Downloaden
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Documentatie
1. [Elastische database splitsen samenvoegen hulpmiddel zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Beveiliging van de gesplitste samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Veiligheidsoverwegingen voor gesplitste samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Beheer van de map shard](sql-database-elastic-scale-shard-map-management.md)
* [Bestaande databases naar schalen migreren](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Hulpmiddelen voor databases elastische](sql-database-elastic-scale-introduction.md)
* [Woordenlijst van de hulpmiddelen voor elastische Database](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Het nut van het hulpmiddel gesplitste samenvoegen?

**Flexibiliteit**

Toepassingen moeten uitrekken flexibel de grenzen van een enkele DB van Azure SQL-database. Gebruik het hulpmiddel om gegevens te verplaatsen naar wens aan nieuwe databases en integriteit behoudt.

**Gesplitste om te laten groeien** 

U moet de algehele capaciteit worden afgehandeld monster groei vergroten. Maak hiervoor extra capaciteit door sharding de gegevens en distribueren over stapsgewijs meer databases totdat de capaciteitsbehoeften is voldaan. Dit is een goed voorbeeld van de functie 'splitsen'. 

**Samenvoegen naar verkleinen**

Capaciteit moet verkleinen vanwege de seizoensgebonden aard van een bedrijf. Het hulpmiddel kunt u de schaal omlaag naar minder schaaleenheden wanneer bedrijven wordt vertraagd. De functie 'samenvoegen' in de Service van de gesplitste samenvoegen elastische schaal behandelt deze vereiste. 

**Hotspots beheren door te shardlets verplaatsen**

De toewijzing van shardlets naar shards kan met meerdere tenants per database leiden tot capaciteit knelpunten op sommige shards. Dit is shardlets opnieuw toewijzen of bezet shardlets verplaatsen naar de nieuwe of minder gebruikte shards vereist. 

## <a name="concepts--key-features"></a>Concepten en belangrijke functies

**Klant gehoste services**

De gesplitste samenvoegen wordt als een klant gehoste service bezorgd. U moet implementeren en de service hosten van uw abonnement op Microsoft Azure. Het pakket dat u van NuGet downloaden bevat een configuratiesjabloon om uit te voeren met de gegevens voor uw specifieke implementatie. Zie de [gesplitste samenvoegen zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md) voor meer informatie. Aangezien de service wordt uitgevoerd in uw Azure-abonnement, kunt u instellen en configureren van de meeste beveiligingsaspecten van de van de service. De standaardsjabloon bevat de opties voor het configureren van SSL, certificaten gebaseerde clientverificatie, versleuteling voor opgeslagen referenties, wat u moet doen beveiligen en IP-beperkingen. U vindt meer informatie over de beveiligingsaspecten in de volgende document [gesplitste samenvoegen beveiliging](sql-database-elastic-scale-split-merge-security-configuration.md).

De standaard geïmplementeerd met één werknemer en één Webrol de service wordt uitgevoerd. Gebruik de grootte A1 VM in Azure-Cloudservices. Terwijl u kunt deze instellingen niet wijzigen wanneer het pakket implementeert, kunt u ze kunnen wijzigen na een geslaagde implementatie in de actieve cloudservice, (via de portal Azure). Houd er rekening mee dat de rol werknemer niet is geconfigureerd voor meer dan één exemplaar voor technische redenen. 

**Integratie van de map shard**

De service gesplitste samenvoegen communiceert met de kaart shard van de toepassing. Wanneer u met de service gesplitste samenvoegen om te splitsen of samenvoegen van bereiken of shardlets tussen shards, wordt de service automatisch de kaart shard up-to-date. Hiervoor de service is verbonden met de shard kaart manager-database van de toepassing en bereiken volgens meerdere toewijzingen onderhoudt als voortgang van het aanvragen van de gesplitste/samenvoegen/verplaatsen. Dit zorgt ervoor dat de kaart shard altijd een actuele weergave presenteert wanneer gesplitste samenvoegbewerkingen terechtkomen op. Split, zijn samenvoegen en shardlet verkeer bewerkingen door een reeks shardlets uit de bron-shard verplaatsen naar de doel-shard geïmplementeerd. Tijdens de shardlet verplaatsing van de shardlets onderhevig aan de huidige batch zijn gemarkeerd als offline in de map shard en zijn niet beschikbaar voor gegevens-afhankelijke verbindingen met de **OpenConnectionForKey** API. 

**Consistente shardlet verbindingen**

Wanneer de verplaatsing van gegevens voor een nieuwe reeks shardlets is gestart, geen shard-overzicht gegevens-afhankelijke routeren verbindingen met het opslaan van de shardlet shard gedode en volgende verbindingen vanuit de kaart shard API's naar de volgende zijn shardlets terwijl de verplaatsing van de gegevens uitgevoerd wordt om te voorkomen inconsistenties worden geblokkeerd. Verbindingen met andere shardlets op de dezelfde shard wordt ook krijgen beëindigd, maar opnieuw slagen direct op opnieuw. Wanneer de batch is verplaatst, wordt de shardlets online opnieuw voor de doeltoepassing shard worden gemarkeerd en wordt de brongegevens is verwijderd uit de bron-shard. Deze stappen voor elke batch doorloopt de service tot alle shardlets zijn verplaatst. Dit leidt tot meerdere verbinding kill bewerkingen in de loop van de volledige gesplitste/samenvoegen/verplaatsen-bewerking.  

**Shardlet beschikbaarheid van beheren**

De verbinding beëindigen naar de huidige groep shardlets zoals hierboven wordt beschreven beperken het bereik van niet beschikbaar, beperkt tot één batch van shardlets tegelijk. Dit heeft de voorkeur boven een aanpak waar de volledige shard blijft offline voor alle bijbehorende shardlets in de loop van een bewerking splitsen of samenvoegen. De grootte van een partij, gedefinieerd als het aantal unieke shardlets verplaatsen per keer, is een configuratieparameter. Dit kan worden gedefinieerd voor elke bewerking samenvoegen en splitsen afhankelijk van de behoeften beschikbaarheid en prestaties van de toepassing. Houd er rekening mee dat het bereik dat is worden vergrendeld in de toewijzing shard mogelijk groter is dan de batch opgegeven. Dit komt omdat de service de grootte van het bereik uitgelicht dat het werkelijke aantal sharding belangrijke waarden in de gegevens ongeveer overeenkomt met de batchgrootte. Dit is belangrijk te weten met name voor zeer gevulde sharding toetsen. 

**Metagegevensopslag**

De service gesplitste samenvoegen gebruikt een database voor het behoud van de status ervan en wilt behouden logboeken tijdens de verwerking van vergaderverzoeken. De gebruiker wordt gemaakt van deze database in hun abonnement en voorziet de verbindingsreeks in het configuratiebestand voor de service-implementatie. Beheerders van de organisatie van de gebruiker kunnen ook verbinding maakt met deze database moet worden gereviseerd voortgang van de aanvraag en gedetailleerde informatie over mogelijke fouten onderzoeken.

**Sharding-chatstatus**

De service gesplitste samenvoegen onderscheidt tussen (1) een laptopgeheugen tabellen, (2) verwijzing tabellen en (3) normale tabellen. De semantiek van een gesplitste/samenvoegen/verplaatsing, hangt af van het type van de tabel die worden gebruikt en worden als volgt gedefinieerd: 

* **Een laptopgeheugen tabellen**: splitsen, samenvoegen en verplaatsen bewerkingen shardlets van bron naar gaan doel shard. Na voltooiing van de algehele aanvraag zijn deze shardlets niet langer aanwezig op de bron. Houd er rekening mee dat het doeltabellen moeten bestaan op de doel-shard en mag geen gegevens in de doelbereik vóór de verwerking van de bewerking. 

* **Verwijzingstabellen**: voor verwijzingstabellen, de splitsing samenvoegen en bewerkingen Kopieer de gegevens uit de bron naar de doel-shard verplaatsen. Bedenk wel dat er geen wijzigingen in de doel-shard voor een bepaalde tabel optreden als een rij al aanwezig zijn in deze tabel op het doel is. De tabel heeft leeg voor elke bewerking verwijzing tabel kopiëren naar verwerkt.

* **Andere tabellen**: andere tabellen kunnen aanwezig zijn op de bron of het doel van een bewerking samenvoegen en splitsen. De service gesplitste samenvoegen negeert deze tabellen gegevens verplaatsen of kopiëren. Bedenk wel dat ze deze bewerkingen voor het geval de beperkingen kunnen storen.

De informatie in verwijzing versus een laptopgeheugen tabellen is verstrekt door de **SchemaInfo** API's op de kaart shard. Het volgende voorbeeld ziet u het gebruik van deze API's op een bepaald shard kaart manager object smm: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

De tabellen 'regio' en 'nationale' worden gedefinieerd als verwijzingstabellen en met gesplitste/samenvoegen/verplaatsen bewerkingen worden gekopieerd. 'klant' en 'orders' worden beurtelings gedefinieerd als een laptopgeheugen tabellen. C_CUSTKEY en O_CUSTKEY fungeren als de sleutel sharding. 

**Referentiële integriteit**

De service gesplitste samenvoegen worden geanalyseerd afhankelijkheden tussen tabellen en refererende sleutels en primaire-sleutelrelaties met de bewerkingen voor het verplaatsen van tabellen en shardlets van fase. In het algemeen, worden verwijzingstabellen gekopieerd eerst in afhankelijkheid volgorde, worden shardlets gekopieerd in de volgorde van de bijbehorende afhankelijkheden binnen elke batch. Dit is nodig zodat FK-PK beperkingen bij de doel-shard worden uitgevoerd als de nieuwe gegevens binnenkomt. 

**Shard kaart consistentie en eventuele voltooiing**

In de aanwezigheid van fouten, de service gesplitste samenvoegen cv's bewerkingen na een storing en ernaar uitvoeren in voortgang aanvragen. Maar er zijn mogelijk onherstelbare situaties, bijvoorbeeld wanneer de doel-shard is verbroken of is gehackt niet meer worden hersteld. In deze omstandigheden blijven sommige shardlets die zijn moet worden verplaatst bevinden zich op de bron-shard. De service zorgt ervoor dat shardlet toewijzingen alleen worden bijgewerkt nadat de benodigde gegevens is gekopieerd naar de doelsite. Shardlets zijn alleen op de bron verwijderd nadat alle hun gegevens is gekopieerd naar de doelsite en de bijbehorende toewijzingen zijn bijgewerkt. Er gebeurt het verwijderen betrekking heeft op de achtergrond terwijl het bereik al online op de doel-shard is. De service gesplitste samenvoegen is altijd zorgt ervoor dat de toewijzingen die zijn opgeslagen in de map shard correct.


## <a name="the-split-merge-user-interface"></a>De gebruikersinterface van gesplitste samenvoegen

Het pakket gesplitste samenvoegen service bevat een rol werknemer en een Webrol. De Webrol wordt gebruikt voor het aanvragen van de gesplitste samenvoegen in een interactieve manier indienen. De belangrijkste onderdelen van de gebruikersinterface zijn als volgt:

-    Bewerkingstype: Het bewerkingstype is een keuzerondje waarmee wordt bepaald van het soort bewerking uitgevoerd door de service voor deze aanvraag. U kunt kiezen tussen de splitsing samenvoegen en scenario's verplaatsen. U kunt ook een eerder ingediende bewerking annuleren. U kunt gesplitste gebruiken, samenvoegen en verzoeken om bereik shard kaarten verplaatsen. Lijst shard kaarten alleen ondersteuning verplaatsen bewerkingen.

-    Shard kaart: Informatie over de kaart shard en de database hostingprovider van uw map shard betrekking hebben op het volgende gedeelte van de aanvraagparameters. Met name, moet u de naam van de Azure SQL Database-server en database de shardmap, referenties verbinding maken met de database shard en ten slotte de naam van de kaart shard hostingprovider. De bewerking accepteert op dit moment alleen één set referenties. Deze referenties moeten voldoende machtigingen om de wijzigingen in de map shard ook dat de gebruikersgegevens uitvoeren op de shards.

-    Bronbereik (splitsen en samenvoegen): een bewerking samenvoegen en splitsen verwerkt een bereik met de onder- en bovendrempel sleutel. Om op te geven een bewerking met een niet-gebonden hoog sleutelwaarde, schakel het selectievakje "hoog sleutel is max" in en laat het veld met hoge sleutel leeg. Het bereik-sleutelwaarden die u opgeeft moet niet precies overeenkomen met een toewijzing en de grenzen in uw map shard. Als u geen de grenzen van elk bereik helemaal opgeeft wordt de service afgeleid het eerstvolgende bereik voor u automatisch. U kunt het GetMappings.ps1 PowerShell-script gebruiken om op te halen van de huidige toewijzingen in een map opgegeven shard.

-    Gedrag van de bron gesplitste (gesplitste): voor gesplitste bewerkingen definiëren op het punt als u wilt het bronbereik splitsen. U doen dit door de gewenste positie voor de splitsing sharding-toets. Gebruik het keuzerondje opgeven of u het onderste gedeelte van het bereik (met uitzondering van de gesplitste-toets) wilt verplaatsen, of of u wilt dat het bovenste gedeelte verplaatsen (inclusief de toets gesplitste).

-    Gegevensbron Shardlet (verplaatsen): verplaatsen bewerkingen is het verschil tussen splitsen of samenvoegen bewerkingen terwijl een bereik om te beschrijven van de bron is niet vereist. Een bron voor verplaatsen is gewoon die wordt aangeduid met het sharding-sleutelwaarde die u wilt verplaatsen.

-    Doelgerichte Shard (gesplitste): nadat u de gegevens hebt opgegeven voor de bron van de gesplitste betrekking heeft, moet u bepalen waar u de gegevens die moeten worden gekopieerd naar doordat de Db van Azure SQL server en database de naam voor het doel.

-    TARGET bereik (samenvoegen): samenvoegbewerkingen shardlets verplaatsen naar een bestaande shard. U kunt de bestaande shard doordat de grenzen van het bereik van het bestaande bereik dat u samenvoegen wilt met aangeven.

-    Batchgrootte: De batch-omvang bepaalt het aantal shardlets die offline tegelijk tijdens de verplaatsing van de gegevens gaat. Dit is een geheel getal waar u kleinere waarden kunt gebruiken wanneer u gevoelige bent in lange perioden van downtime voor shardlets. Hogere waarden, neemt de tijd die een opgegeven shardlet is offline maar kan de prestaties verbeteren.

-    Bewerking-Id (annuleren): Als u een bewerking die is niet meer nodig hebt, kunt u de bewerking annuleren door op te geven van de bewerkings-ID in dit veld. U kunt de bewerkings-ID ophalen uit de tabel met verzoek om status (Zie sectie 8.1) of vanuit de uitvoer in de webbrowser waar u de aanvraag hebt verzonden.


## <a name="requirements-and-limitations"></a>Vereisten en beperkingen 

De huidige implementatie van de gesplitste samenvoegen-service is onderhevig aan de volgende vereisten en beperkingen: 

* De shards moeten bestaan en worden geregistreerd in de toewijzing shard voordat een gesplitste-samenvoegbewerking op deze shards kan worden uitgevoerd. 

* De service maakt geen tabellen of andere databaseobjecten automatisch als onderdeel van de bewerkingen. Dit betekent dat het schema voor alle een laptopgeheugen tabellen en verwijzing moet bestaan op de doel-shard voordat u een bewerking samenvoegen/splitsen/verplaatsen. Een laptopgeheugen tabellen zijn met name moeten in het bereik die waar de nieuwe shardlets moet worden toegevoegd door een gesplitste/samenvoegen/verplaatsing zijn niet leeg zijn. De bewerking, anders de eerste consistentiecontrole van de doel-shard mislukt. Bedenk ook dat gegevens worden alleen gekopieerd als de verwijzing tabel leeg is en dat er geen consistentie garanties met betrekking tot andere gelijktijdige bewerkingen op de verwijzingstabellen schrijven verwijzing. Het is raadzaam dit: wanneer gesplitste/samenvoegen bewerkingen uitgevoerd, geen andere schrijven-bewerkingen wijzigingen aanbrengen in de verwijzingstabellen.

* De service is afhankelijk van rij identiteit tot stand gebracht door een unieke index of sleutel waarin de sharding-toets om te verbeteren de prestaties en betrouwbaarheid voor grote shardlets. Hierdoor wordt de service om gegevens te verplaatsen op een zelfs weer specifieker dan alleen de sleutelwaarde sharding. Hiermee kunt u de maximale hoeveelheid ruimte en vergrendelingen die vereist tijdens de bewerking zijn beperken. Kunt u een unieke index of een primaire sleutel, inclusief de sharding-toets op een bepaalde tabel als u wilt gebruiken die tabel met gesplitste/samenvoegen/verplaatsen aanvragen maken. Prestatieoverwegingen moet de sleutel sharding bestaan uit de eerste kolom in de toets of de index.

* In de loop van de verwerking van vergaderverzoeken, kan sommige gegevens shardlet aanwezig zowel op de bron en de doel-shard zijn. Dit is nodig om te beveiligen tegen fouten tijdens het verkeer shardlet. De integratie van gesplitste-samenvoegen met de kaart shard zorgt ervoor dat verbindingen via de afhankelijke routering API's met de methode **OpenConnectionForKey** op de kaart shard gegevens een inconsistente tussenliggende Staten niet ziet. Wanneer u verbinding maakt met de bron- of de doel-shards zonder de methode **OpenConnectionForKey** , inconsistente tussenliggende Staten mogelijk wel zichtbaar wanneer gesplitste/samenvoegen/verplaatsen aanmeldingsaanvragen kunnen worden verzonden op. Deze verbindingen mogelijk gedeeltelijke of dubbele resultaten afhankelijk van de tijdsinstellingen of de onderliggende de verbinding shard weergeven. Deze beperking bevat momenteel de verbindingen die zijn aangebracht door elastische schaal meerdere / Shard-query's.

* De metagegevensdatabase voor de service gesplitste samenvoegen moet tussen verschillende rollen niet worden gedeeld. Bijvoorbeeld een rol van de gesplitste samenvoegen service wordt uitgevoerd in tijdelijke behoeften zodat deze verwijzen naar een andere metagegevensdatabase dan de rol productie.
 

## <a name="billing"></a>Facturering 

De gesplitste samenvoegen-service wordt uitgevoerd als een cloudservice in uw Microsoft Azure-abonnement. Daarom kosten voor cloudservices gelden voor uw exemplaar van de service. Tenzij u vaak gesplitste/samenvoegen/verplaatsen bewerkingen uitvoert, wordt u aangeraden dat u uw gesplitste samenvoegen cloudservice verwijderen. Die worden opgeslagen kosten voor uitgevoerd of exemplaren van de cloud-service geïmplementeerd. U kunt opnieuw te implementeren en uw gemakkelijk runnable configuratie starten wanneer u wilt splitsen of samenvoegen bewerkingen uitvoeren. 
 
## <a name="monitoring"></a>Cmdlets voor controle 
### <a name="status-tables"></a>Status tabellen 

De gesplitste samenvoegen-Service biedt de **RequestStatus** -tabel in de metagegevens opslaan database voor het controleren van voltooide en continue aanvragen. De tabel bevat een rij voor elke gesplitste samenvoegen aanvraag die op dit exemplaar van de gesplitste samenvoegen-service is ingediend. De volgende informatie voor elke aanvraag hebt:

* **Tijdstempel**: de tijd en datum waarop de aanvraag is gestart.

* **Bewerkingsnummer**: een GUID die deze identificeert op het verzoek. Deze aanvraag kan ook worden gebruikt de bewerking te annuleren terwijl het is nog steeds lopende.

* **Status**: de huidige status van de aanvraag kunt invullen. Voor lopende aanvragen worden deze ook de huidige fase waarin de aanvraag is.

* **CancelRequest**: een vlag waarmee wordt aangegeven of de aanvraag is geannuleerd.

* **Voortgang**: een geschatte percentage voltooid voor de bewerking. Een waarde van 50 geeft aan dat de bewerking ongeveer 50% is voltooid.

* **Meer informatie**: een XML-waarde die een meer gedetailleerde voortgangsrapport biedt. Het voortgangsrapport wordt regelmatig bijgewerkt terwijl sets met rijen worden gekopieerd van bron naar doel. Fouten of uitzonderingen bevat deze kolom ook meer gedetailleerde informatie over de fout.


### <a name="azure-diagnostics"></a>Azure diagnostische gegevens

Hulpprogramma's voor diagnose Azure op basis van Azure SDK 2,5 voor controle en diagnostische hulpprogramma's wordt gebruikt door de service gesplitste samenvoegen. U de configuratie diagnostische hulpprogramma's beheren, zoals hier wordt uitgelegd: [Diagnostische hulpprogramma's inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md). Het pakket bevat twee hulpprogramma's voor diagnose configuraties – één voor de Webrol en één voor de werknemer rol. Deze configuraties diagnostische hulpprogramma's voor de service volgt u de richtlijnen van [Grondbeginselen van Cloud-Service in Microsoft Azure wordt aangegeven](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Het bevat de definities om aan te melden prestatie-items, IIS logboeken gebeurtenislogboeken van Windows en gebeurtenislogboeken van gesplitste samenvoegen toepassingen. 

## <a name="deploy-diagnostics"></a>Diagnostische gegevens implementeren 

Als u de cmdlets voor controle en diagnostische gegevens met behulp van de diagnostische configuratie voor het web en werknemer rollen verstrekt door het pakket NuGet, voert u de volgende opdrachten via Azure PowerShell: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

U vindt meer informatie over het configureren en implementeren van hier de instellingen voor de diagnostische gegevens: [Diagnostische hulpprogramma's inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Diagnostische gegevens ophalen 

U kunt eenvoudig uw diagnostische hulpprogramma's openen vanuit de Visual Studio Server Explorer in het Azure deel van de Server Explorer-structuur. Open een exemplaar van de Visual Studio en klik in de menubalk op weergave en Server Explorer. Klik op het pictogram Azure verbinding maken met uw Azure-abonnement. Ga naar Azure -> opslag -> <your storage account> -> tabellen -> WADLogsTable. Zie [Opslagruimte Resources bladeren met Explorer Server](http://msdn.microsoft.com/library/azure/ff683677.aspx)voor meer informatie. 

![WADLogsTable][2]

De WADLogsTable gemarkeerd in de bovenstaande afbeelding bevat de gedetailleerde gebeurtenissen uit van de gesplitste samenvoegen service-toepassingslogboek. Houd er rekening mee dat de standaard-configuratie van het gedownloade pakket is gericht op een productie-implementatie. Het interval waarop Logboeken en items worden opgehaald uit de service-exemplaren dus grote (5 minuten). Voor tests en ontwikkeling, Verlaag het interval door de instellingen voor diagnostische gegevens van het web of de rol werknemer aan uw behoeften aan te passen. Met de rechtermuisknop op de rol in de Visual Studio Server Explorer (Zie boven) en past u de periode overbrengen in het dialoogvenster voor de hulpprogramma's voor diagnose configuratie-instellingen: 

![Configuratie][3]


## <a name="performance"></a>Prestaties

Betere prestaties is in het algemeen wordt naar verwachting uit een hogere, meer zodat Servicelagen in Azure SQL-Database. Hogere IO, processor en geheugen toewijzingen voor de hogere niveaus van de service profiteren van de kopie bulksgewijs en bewerkingen die gebruikmaakt van de gesplitste samenvoegen-service verwijderen. Om die reden de servicelaag alleen betrekking heeft op die databases voor een gedefinieerde, beperkte periode te vergroten.

De service kunt u ook validatie query's als onderdeel van de normale bewerkingen uitvoeren. Deze validatie query's controleren op onverwachte aanwezigheid van gegevens in de doelbereik en zorg ervoor dat alle gesplitste/samenvoegen/verplaatsing van een consistente status wordt gestart. Deze alle query's werken via sharding belangrijke bereiken gedefinieerd door het bereik van de bewerking of de batchgrootte die is opgegeven als onderdeel van de aanvraag definitie. Deze query's uitvoeren best wanneer een index is presenteren met de toets sharding als de eerste kolom. 

Bovendien kan een eigenschap uniekheid met de toets sharding als de eerste kolom de service voor het gebruik van een geoptimaliseerde aanpak die resourceverbruik, in het logboek met spaties en geheugen beperkt. Deze eigenschap uniekheid is vereist voor het verplaatsen van grote gegevens grootte (meestal boven 1GB). 

## <a name="how-to-upgrade"></a>Upgrade uitvoeren

1. Volg de stappen in [een service gesplitste samenvoegen Deploy](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Het configuratiebestand cloud-service voor uw implementatie gesplitste samenvoegen om weer te geven van de configuratieparameters voor de nieuwe wijzigen. Een nieuwe vereiste parameter is de informatie over het certificaat voor versleuteling. Een eenvoudige manier hiervoor is het nieuwe configuratie-sjabloonbestand van het downloaden ten opzichte van de configuratie van uw bestaande vergelijken. Controleer of dat u de instellingen voor "DataEncryptionPrimaryCertificateThumbprint" en "DataEncryptionPrimary" voor zowel het web en de rol werknemer toevoegen.
3. Zorg ervoor dat alle lopende gesplitste samenvoegen bewerkingen zijn voltooid voordat de update naar Azure wordt geïmplementeerd. U kunt dit eenvoudig doen door de query's uitvoeren de tabellen RequestStatus en PendingWorkflows in de metagegevensdatabase splitsen samenvoegen voor lopende aanvragen.
4. Werk uw bestaande cloud service implementatie van gesplitste-samenvoegen in uw Azure-abonnement met het nieuwe pakket en het configuratiebestand bijgewerkte service.

U hoeft niet voor het inrichten van een nieuwe metagegevensdatabase voor gesplitste-samenvoegen upgrade uit te voeren. De nieuwe versie wordt de bestaande metagegevensdatabase automatisch bijgewerkt naar de nieuwe versie. 

## <a name="best-practices--troubleshooting"></a>Aanbevolen procedures en probleemoplossing
 
-    Een test-tenant definiëren en uw belangrijkste bewerkingen gesplitste/samenvoegen/verplaatsen met de toets tenant oefenen over verschillende shards. Zorg ervoor dat alle metagegevens correct is gedefinieerd in uw map shard en dat de bewerkingen niet worden geschonden beperkingen of refererende sleutels.
-    Houd de toets tenant problemen met de gegevensgrootte boven de maximale grootte van de grootste tenant zodat u de gegevensgrootte van de niet doe. Hiermee kunt u een bovengrens beoordelen op hoe lang die het duurt een enkel tenant bewegen. 
-    Zorg ervoor dat in uw schema verwijderingen is toegestaan. De gesplitste samenvoegen-service vereist de mogelijkheid om gegevens te verwijderen uit de bron-shard nadat de gegevens is gekopieerd naar de doelsite. Bijvoorbeeld, **Verwijder triggers** kunt voorkomen dat de service verwijderen van de gegevens op de bron en bewerkingen mislukken kan veroorzaken.
-    De sleutel sharding moet bestaan uit de eerste kolom in de primaire sleutel of unieke indexdefinitie. Die zorgt ervoor dat de beste prestaties voor de splitsen of samenvoegen validatie-query's en voor de werkelijke verplaatsing en verwijdering gegevensbewerkingen die altijd werken op sharding belangrijke bereiken.
-    Uw service gesplitste samenvoegen in de regio en Datacenter waar uw databases woont collocate. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 
