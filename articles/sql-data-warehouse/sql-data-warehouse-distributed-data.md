<properties
   pageTitle="Gedistribueerde gegevens en distributed Tabelopties voor de Massively parallelle Processing (MPP)-mailsystemen van SQL Data Warehouse en Parallel Data Warehouse | Microsoft Azure"
   description="Leer hoe gegevens worden gedistribueerd voor Massively parallelle Processing (MPP) en de opties voor het distribueren van tabellen in Azure SQL Data Warehouse en Parallel Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Gedistribueerde gegevens en verdeelde tabellen voor Massively parallelle Processing (MPP)

Leer hoe gebruikersgegevens wordt gedistribueerd in Azure SQL Data Warehouse en Parallel Data Warehouse, die Microsofts Massively parallelle Processing MPP-systemen. Ontwerpen van uw gegevenswarehouse gedistribueerde gegevens effectief gebruiken, helpt u bij het bereiken van de voordelen van de architectuur MPP verwerken van query's. Een paar database ontwerpkeuzen kunnen belangrijke gevolgen voor het verbeteren van de prestaties van query's hebben.  

>[AZURE.NOTE] Azure SQL Data Warehouse en Parallel Data Warehouse gebruiken hetzelfde Massively parallelle Processing MPP-ontwerp, maar er enkele verschillen vanwege het onderliggende platform. SQL Data Warehouse is een Platform als een Service (PaaS) die wordt uitgevoerd op Azure. Parallel Data Warehouse wordt uitgevoerd op Analytics Platform systeem (AP) dat wil zeggen een on-premises implementatie toestel die op Windows Server wordt uitgevoerd.

## <a name="what-is-distributed-data"></a>Wat zijn gedistribueerde gegevens?

In SQL Data Warehouse en Parallel Data Warehouse verwijst gedistribueerde gegevens naar de gegevens van de gebruiker die is opgeslagen op meerdere locaties in het MPP-systeem. Elk van deze locaties fungeert als een onafhankelijke opslag en verwerkingseenheid die wordt uitgevoerd van query's op het gedeelte van de gegevens. Gedistribueerde gegevens zijn fundamenteel voor query's uitvoeren om te bereiken hoog queryprestaties parallel.

Als u wilt verspreiden gegevens, wordt elke rij van de gebruikerstabel van een in datawarehouse toegewezen naar één gedistribueerde locatie.  U kunt tabellen met een hash-verdeling-methode of een methode round robin distribueren. De verdelingsmethode is opgegeven in de instructie CREATE TABLE. 

## <a name="hash-distributed-tables"></a>Tabellen hash distributed
  
In het volgende diagram ziet u hoe een volledige (niet-distributed tabel) wordt opgeslagen als een tabel hash verdeeld. Een deterministic functie elke rij wilt toevoegen aan één verdeling toegewezen. Bij de definitie van de tabel, een van de kolommen aangewezen als de kolom verdeling. De hash-functie gebruikt de waarde in de kolom verdeling elke rij toewijzen aan een verdeling.

Er zijn Prestatieoverwegingen voor de selectie van de kolom van een verdeling, zoals onderscheidbaarheid, gegevens scheefheid en de typen query's uitvoeren op het systeem.
  
![Verdeeld tabel] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Verdeeld tabel")  
  
-   Elke rij hoort bij een verdeling.  
  
-   Elke rij een deterministic hashalgoritme toegewezen aan één verdeling.  
  
-   Het aantal rijen per verdeling verschilt zoals aangegeven door de verschillende grootte van tabellen.

## <a name="round-robin-distributed-tables"></a>Round robin verdeelde tabellen

Een round robin verdeelde tabel distribueert de rijen op elke rij toewijzen aan een distributiegroep in de juiste volgorde. Het is snel gegevens in een tabel round robin laden, maar het is mogelijk dat queryprestaties langzamer afspelen.  Lid worden van een tabel round robin meestal vereist reshuffling de rijen om te schakelen van de query toevoegen aan een nauwkeurige resultaat, die duurt produceren.

## <a name="distributed-storage-locations-are-called-distributions"></a>Verdeelde opslaglocaties heten onderzoeken

Elke verdeelde locatie wordt een verdeling genoemd. Wanneer een query wordt uitgevoerd parallel, uitvoeren elke verdeling met een SQL-query het gedeelte van de gegevens. SQL-Database SQL Data Warehouse gebruikt de query uit te voeren. SQL Server Parallel Data Warehouse gebruikt de query uit te voeren. Het architectuurontwerp van deze gedeelde is fundamentele bij het bereiken van schalen parallelle computing.

### <a name="can-i-view-the-distributions"></a>Kan ik de onderzoeken weergeven?

Elke verdeling heeft een verdeling-ID en is zichtbaar in systeemweergaven die betrekking op SQL Data Warehouse en Parallel Data Warehouse hebben. U kunt de ID van de verdeling prestaties van query's en andere problemen oplossen. Voor een lijst van de systeemweergaven, ziet u de [weergave van MPP-systeem](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Verschil tussen een verdeling en een berekeningscluster knooppunt

Een verdeling wordt de basiseenheid is voor het opslaan van gedistribueerde gegevens en parallelle query's worden verwerkt. Onderzoeken zijn in berekeningscluster knooppunten gegroepeerd. Elk knooppunt berekeningscluster houdt een of meer onderzoeken.  

-   Berekeningscluster knooppunten Platform analysesysteem gebruikt als een centraal onderdeel van de mogelijkheden van architectuur en schalen hardware. Altijd gebruikt de functie acht onderzoeken per berekeningscluster knooppunt, zoals wordt weergegeven in het diagram voor tabellen hash verdeeld. Het aantal knooppunten berekenen en kunnen daarom het aantal onderzoeken, wordt bepaald door het aantal berekeningscluster knooppunten die u voor het toestel kopen. Als u acht berekeningscluster knooppunten hebt aangeschaft, krijgt u bijvoorbeeld 64 onderzoeken (8 berekeningscluster knooppunten x 8 onderzoeken/knooppunt). 

-   SQL Data Warehouse heeft een vast aantal 60 onderzoeken en een flexibele aantal knooppunten berekeningscluster. De knooppunten berekenen worden met Azure computing en opslagruimte resources geïmplementeerd. Het aantal knooppunten berekenen kunt op basis van de werklast van de service backend en de computer capaciteit (DWUs) die u voor het datawarehouse wijzigen. Wanneer het aantal knooppunten berekeningscluster verandert, wordt het aantal onderzoeken per berekeningscluster knooppunt ook gewijzigd. 

### <a name="can-i-view-the-compute-nodes"></a>Kan ik de knooppunten berekeningscluster weergeven?

Elk knooppunt berekeningscluster heeft een knooppunt-ID en is zichtbaar in systeemweergaven die betrekking op SQL Data Warehouse en Parallel Data Warehouse hebben.  U kunt het knooppunt berekeningscluster zien door te zoeken naar de kolom node_id in systeemweergaven waarvan de naam met sys.pdw_nodes begint. Voor een lijst van de systeemweergaven, ziet u de [weergave van MPP-systeem](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Tabellen voor Parallel datawarehouse gerepliceerd 
  
Van toepassing op: Parallel datawarehouse

Parallel Data Warehouse biedt naast verdeelde tabellen, een optie voor het repliceren tabellen. Een *tabel gerepliceerd* is een tabel die is opgeslagen in zijn geheel op elk knooppunt berekeningscluster. Repliceren van een tabel verwijdert dat u de tabelrijen tussen berekeningscluster knooppunten vóór het gebruik van de tabel in een join of aggregatie overbrengen. Gerepliceerde tabellen zijn alleen mogelijk gecombineerd met kleine tabellen vanwege de extra opslagruimte voor de opslag van de volledige tabel op elk knooppunt berekeningscluster is vereist.  
  
In het volgende diagram ziet u een gerepliceerde tabel die is opgeslagen op elk knooppunt berekeningscluster. De gerepliceerde tabel is over alle schijven die zijn toegewezen aan het knooppunt berekeningscluster opgeslagen. Deze strategie schijf wordt geïmplementeerd met behulp van SQL Server-bestandsgroepen.  
  
![Gerepliceerde tabel] (media/sql-data-warehouse-distributed-data/replicated-table.png "Gerepliceerde tabel") 
  
## <a name="next-steps"></a>Volgende stappen
  
Verdeelde tabellen als effectief wilt gebruiken, raadpleegt u [distribueren tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md)  
  



  
