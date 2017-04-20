<properties
   pageTitle="Databases in Azure SQL Data Warehouse beheren | Microsoft Azure"
   description="Overzicht van de SQL Data Warehouse databases beheren. Hulpmiddelen voor projectbeheer, DWUs en schalen prestaties, probleemoplossing queryprestaties, goede beveiligingsbeleid voor apparaten tot stand te brengen en een database herstellen vanuit beschadiging of een regionale storing bevat."
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
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Databases in Azure SQL Data Warehouse beheren

SQL Data Warehouse automatiseert veel aspecten van uw databases beheren. Bijvoorbeeld als u wilt schalen prestaties moet alleen u aanpassen en laat SQL Data Warehouse het werk van schalen en schaalbaarheid terug betalen voor het juiste niveau berekeningscluster resources. 

Ongetwijfeld wilt controleren van uw werkzaamheden als u wilt identificeren de prestatiebehoeften van uw, evenals langdurige query's oplossen. U moet ook een paar beveiligingstaken uitvoeren om te machtigingen beheren voor gebruikers en aanmeldingen.

Dit overzicht worden deze aspecten van SQL Data Warehouse beheren.

- Hulpmiddelen voor projectbeheer
- Schaal berekenen
- Onderbreken en hervatten
- Aanbevolen procedures voor prestaties
- Query controleren
- Beveiliging
- Back-up en herstellen

## <a name="management-tools"></a>Hulpmiddelen voor projectbeheer

U kunt diverse hulpprogramma's voor het beheren van databases in SQL Data Warehouse gebruiken. Wanneer u databases beheert, wordt u ontwikkelen hulpmiddel voorkeuren voor elk type taak dat u wilt uitvoeren.

### <a name="azure-portal"></a>Azure-portal
De [Azure-portal][] is een web-portal waar u kunt maken, bijwerken, en verwijderen van databases en controleren database bronnen. Dit hulpmiddel is handig is als u gaat voor het eerst met Azure, het beheren van een klein aantal data warehouse-databases, of als u wilt snel bewerkingen uitvoeren.

Zie aan de slag met Azure portal [maken een SQL Data Warehouse (Azure portal)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools in Visual Studio
[SQL Server Data Tools][] (SSDT) in Visual Studio, kunt u verbinding maakt met, beheren en ontwikkelen van uw databases. Als u een ontwikkelaar van toepassingen werken met Visual Studio of andere geïntegreerde ontwikkelomgeving biedt (IDEs), kunt u SSDT in Visual Studio gebruiken.

SSDT bevat de SQL Server Object Explorer waarmee u kunt visualiseren, verbinding maken en uitvoeren van scripts op SQL Data Warehouse databases. Als u wilt snel een verbinding maken met SQL Data Warehouse, klikt u eenvoudigweg op de knop **openen in Visual Studio** in de opdrachtenbalk bij het weergeven van de informatie over de database in de klassieke Azure-Portal.  

Zie [Query Azure SQL Data Warehouse met Visual Studio][]om te beginnen met SSDT in Visual Studio.

### <a name="command-line-tools"></a>Opdrachtregel hulpmiddelen
Hulpmiddelen voor de opdrachtregel zijn ideaal voor het automatiseren van uw werkbelasting.  PowerShell en sqlcmd zijn twee goede manieren om uw processen te automatiseren.  Het is raadzaam deze hulpprogramma's voor een groot aantal logische servers beheren en implementeren van resource wijzigingen in een productieomgeving, zoals de taken die nodig kunnen worden vastgelegd in een script en vervolgens wordt automatisch in.

### <a name="dynamic-management-views"></a>Dynamische management weergaven 

DMVs zijn de brood en boter van het beheren van SQL Data Warehouse. Bijna alle informatie in de portal waarmee, is afhankelijk van DMVs. Een lijst met SQL Data Warehouse DMVs, raadpleegt u [SQL Data Warehouse systeemweergaven][].

Zie [verbinding maken en de query met sqlcmd][]en [een database (PowerShell) maken][]om te beginnen.

## <a name="scale-compute"></a>Schaal berekenen

In SQL Data Warehouse, kunt u snel de prestaties af of terug door oplopende of aflopende volgorde berekeningscluster bronnen van processor en geheugen o bandbreedte schaal. Als prestaties wilt verkleinen, enige u moet doen, is het aantal data warehouse eenheden (DWUs) die SQL Data Warehouse worden toegewezen aan uw database aanpassen. SQL Data Warehouse snel zorgt ervoor dat de wijziging en alle onderliggende wijzigingen in hardware en software verwerkt.

Zie meer informatie over de schaalbaarheid van DWUs [schaal prestaties][].

##  <a name="pause-and-resume"></a>Onderbreken en hervatten

Als u wilt kosten hebt opgeslagen, kunt u onderbreken en hervatten van berekeningscluster resources op de aanvraag. Bijvoorbeeld als u niet de database nachts en in het weekend gebruikt, kunt u tijdens deze tijden onderbreken en hervatten gedurende de dag. U geen afgeschreven voor DWUs terwijl de database is onderbroken.

Zie [berekenen onderbreken][]en [hervatten berekenen][]voor meer informatie.

## <a name="performance-best-practices"></a>Aanbevolen procedures voor prestaties

Wanneer aan de slag met een nieuwe technologie, bespaart ontdekken de tips en trucs die aanbevolen rechts van de begindatum werken u veel tijd.  U vindt u aanbevelingen overal in veel van onze onderwerpen.

Veel een samenvatting van de belangrijkste overwegingen bij het ontwikkelen van uw werkzaamheden, raadpleegt u [SQL Data Warehouse aanbevolen procedures][].

## <a name="query-monitoring"></a>Query controleren

Soms een query te lang is uitgevoerd, maar u niet weet welke is klikken. SQL Data Warehouse heeft dynamische management weergaven (DMVs) die u gebruiken kunt om te bepalen welke query te lang duurt. 

Langdurige query's, Zie [uw werkzaamheden met DMVs controleren][].

## <a name="security"></a>Beveiliging

Voor het behoud van een beveiligd systeem moet u zich op de melding en bescherming tegen elk gewenst type onbevoegde toegang. Een beveiligingssysteem nodig heeft om ervoor te zorgen dat firewallregels zijn op hun plaats staan, zodat alleen geautoriseerd IP-adressen verbinding kunnen maken. Deze de juiste verificatie van gebruikersreferenties nodig. Nadat een gebruiker is verbonden met de database, moet de gebruiker alleen machtigingen voor het uitvoeren van een minimum aantal acties hebben. U kunt versleuteling gebruiken om gegevens te beveiligen. Het is ook van belang dat u over het bijhouden en controleren zodat u gebeurtenissen keren kunt als er een verdachte activiteit.

Voor meer informatie over het beheren van beveiliging en hoofd boven aan het [overzicht van de beveiliging][].

## <a name="backup-and-restore"></a>Back-up en herstellen

Betrouwbare backps van uw gegevens met een essentiële deel uitmaakt van een productiedatabase. SQL Data Warehouse uw gegevens houdt veilig door automatisch back-ups van uw actieve databases met regelmatige tussenpozen. Deze back-ups kunnen u uit scenario's waarin u hebt uw gegevens beschadigd of per ongeluk verwijderd van uw gegevens of de database herstellen.  Zie [herstellen van momentopname][]voor de back-planning van de gegevens, bewaarbeleid verplicht te maken en hoe u een database, herstelt.

## <a name="next-steps"></a>Volgende stappen
Goed databaseontwerp beginselen wordt het gemakkelijker maken voor het beheren van uw databases in SQL Data Warehouse gebruiken. Meer informatie, hoofd boven aan het [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[Een SQL datawarehouse (Azure Portal) maken]: sql-data-warehouse-get-started-provision.md
[Een database (PowerShell) maken]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL datawarehouse met Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Verbinding maken en de query met sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md
[Uw werkzaamheden met DMVs controleren]: sql-data-warehouse-manage-monitor.md
[Een pauze invoegen berekenen]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Terugzetten vanuit momentopname]: sql-data-warehouse-restore-database-overview.md
[Cv berekenen]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Prestaties van de schaal]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Beveiligingsoverzicht]: sql-data-warehouse-overview-manage-security.md
[Aanbevolen procedures voor Warehouse SQL-gegevens]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse systeemweergaven]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure-portal]: http://portal.azure.com/
