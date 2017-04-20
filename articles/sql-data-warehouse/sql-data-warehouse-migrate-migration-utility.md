<properties
   pageTitle="Migreren: Datawarehouse migratiehulpprogramma | Microsoft Azure"
   description="Migreren naar SQL datawarehouse."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Data Warehouse migratiehulpprogramma (Preview)

> [AZURE.SELECTOR]
- [Migratiehulpprogramma downloaden][]

Het hulpprogramma Data Warehouse migratie is een hulpmiddel ontworpen voor het migreren van schema en de gegevens uit SQL Server en Azure SQL-Database naar Azure SQL Data Warehouse. Tijdens de migratie van schema, het hulpmiddel automatisch toegewezen voor het bijbehorende schema vanaf bron naar de bestemming. Nadat het schema zijn gemigreerd, biedt de hulpmiddelen voor de optie om gegevens met de automatisch gegenereerde scripts te verplaatsen.

Naast het schema en de gegevens migratie kunt met dit hulpmiddel u de optie voor het genereren van compatibiliteitsrapporten waarin compatibiliteitsproblemen tussen de doel- en exemplaren waardoor gestroomlijnde migratie samenvatten.

## <a name="get-started"></a>Aan de slag
Als een vereiste voor installatie moet u het hulpprogramma BCP Office om weer te geven het compatibiliteitsrapport en migratiescripts uitvoeren. Na het starten van het uitvoerbare bestand dat is gedownload wordt u gevraagd een standaard EULA accepteren voordat het hulpmiddel wordt geïnstalleerd.

Daarnaast wilt uitvoeren door de migratie-Utiliy, moet u de machtigingen volgen op de database die u op zoek bent om te migreren: DATABASE maken, een DATABASE wijzigen of de definitie van weergave.

### <a name="launching-the-tool-and-connecting"></a>Het hulpmiddel starten en verbinding maken
Het hulpmiddel starten door te klikken op het pictogram van het bureaublad dat wordt weergegeven bericht installeren. Bij het openen van het hulpmiddel, wordt u gevraagd met een eerste verbinding tot stand pagina waarin u uw bron- en doeltabellen voor het Migratiehulpmiddel uitvoert kunt kiezen. Op dit moment kan ondersteunen we SQL Server en Azure SQL-Database als bronnen en SQL Data Warehouse als doel. Nadat u dit hebt wordt u gevraagd te verbinden met uw bronserver vult servernaam en verifiëren en klik vervolgens op 'Verbinding'.

Nadat u hebt geverifieerd, wordt het hulpmiddel een lijst met databases die aanwezig in de server waarop u zijn met verbonden bent weergegeven. U kunt de migratie beginnen door te selecteren van een database die u wilt migreren en vervolgens te klikken op 'Migreren geselecteerd'.

## <a name="migration-report"></a>Migratierapport
'Databasecompatibiliteit controleren' selecteren in het hulpmiddel genereert een rapport met overzicht van alle object compatibiliteitsproblemen in de database dat u wilt overzetten. Een uitgebreidere lijst met enkele van de SQL Server-functionaliteit die is niet aanwezig zijn in SQL Data Warehouse vindt u in de [documentatie van de migratie][]. Nadat het rapport wordt gegenereerd is mogelijk op te slaan en het rapport openen in Excel.

Houd er rekening mee dat bij het genereren van de migratie-schema, de meeste problemen aangewezen als 'Object' wordt aangepast zodat onmiddellijke migratie van die gegevens. Raadpleeg de wijzigingen zodat u niet wilt extra wijzigingen aanbrengen voordat het schema is toegepast.

## <a name="migrate-schema"></a>Schema migreren

Nadat u verbinding maakt, selecteren 'Schema migreren' wordt een schema migratie script genereren voor de geselecteerde tabellen. In dit script-poorten de structuur van de tabel, kaarten incompatibele gegevens typt tot meer compatibele formulieren en beveiligingsreferenties en schema gemaakt als dit wordt aangegeven door de gebruiker in de migratie-instellingen. Deze code ten opzichte van het gerichte SQL Data Warehouse exemplaar kan worden uitgevoerd, opgeslagen in een bestand, naar het Klembord hebt gekopieerd of zelfs bewerkt in line alvorens verdere actie te ondernemen.  

Anders is vermeld als hierboven, dat wanneer migreren schema controleren de migratie die verandert het hulpmiddel heeft ingesteld om te waarborgen dat dat u volledig begrijpen.  

## <a name="migrate-data"></a>Migreren van gegevens

U kunt BCP-scripts die wordt uw gegevens eerst naar platte bestanden op uw server verplaatsen, genereren door te klikken op de optie 'Gegevens migreren' en kiest u vervolgens rechtstreeks in uw SQL Data Warehouse. Het is raadzaam dit proces voor het verplaatsen van kleine hoeveelheden gegevens en, als nieuwe pogingen niet zijn ingebouwd en er kunnen optreden als er een verlies van de netwerkverbinding. Om te kunnen uitvoeren, moet u hebt het BCP hulpprogramma geïnstalleerd en het schema voor de gegevens moet al hebt gemaakt.

Nadat u de bovenstaande parameters hebt ingevuld gewoon moet u op uitvoeren migratie en een reeks twee pakketten op de opgegeven locatie wordt gegenereerd. Het exportbestand uitvoeren om te kunnen gegevens exporteren uit de migratiebron naar platte bestanden en het importbestand uitvoeren om te kunnen uw gegevens importeren in SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen
Nu dat u bepaalde gegevens hebt gemigreerd, raadpleegt u het [ontwikkelen van][].

<!--Image references-->

<!--Article references-->
[migratie documentatie]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Migratiehulpprogramma downloaden]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
