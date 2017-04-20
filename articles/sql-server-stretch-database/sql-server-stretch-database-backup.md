<properties
    pageTitle="Back-up maken van databases uitrekken ingeschakelde | Microsoft Azure"
    description="Leer hoe u een back-up uitrekken\-databases ingeschakeld."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Back-up uitrekken ingeschakelde databases

Back-ups helpt u bij het herstellen van verschillende soorten fouten, fouten en systeemfouten.  

-   U moet back-up van uw uitrekken\-ingeschakeld van SQL Server-databases.  

-   Microsoft Azure automatisch een back-up van de externe gegevens die uitrekken Database van SQL Server heeft gemigreerd naar Azure.  

>    [AZURE.NOTE] Back-up is slechts één onderdeel van een volledige beschikbaarheid en bedrijfscontinuïteit bedrijfsoplossing. Zie voor meer informatie over de beschikbaarheid, [Hoge beschikbaarheid oplossingen](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>Back-up van uw SQL Server-gegevens  

Back-up uw uitrekken\-ingeschakeld van SQL Server-databases, u kunt doorgaan met het gebruik van de back-up SQL Server-methoden die u momenteel gebruikt. Zie [een Back-Up en herstellen van SQL Server-Databases](https://msdn.microsoft.com/library/ms187048.aspx)voor meer informatie.

Back-ups van een uitrekken ingeschakelde SQL Server-database bevatten alleen lokale gegevens en gegevens in aanmerking komende migratie op het punt in de tijd waarop de back-up wordt uitgevoerd. \(Gegevens die in aanmerking komend zijn gegevens die nog niet zijn gemigreerd, maar worden gemigreerd naar Azure op basis van de migratie-instellingen van de tabellen.\) Hiermee wordt bekend als een **recente** back-up en is niet inbegrepen bij de gegevens al zijn gemigreerd naar Azure.  

## <a name="back-up-your-remote-azure-data"></a>Een back-up van uw externe Azure-gegevens   

Microsoft Azure automatisch een back-up van de externe gegevens die uitrekken Database van SQL Server heeft gemigreerd naar Azure.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure Hiermee reduceert u de kans op pesterijen gegevensverlies met automatische back-up  
De service uitrekken van SQL Server-Database op Azure beschermen uw externe databases met automatische opslag momentopnamen op minstens elke 8 uur. Elke momentopname zeven dagen u voorziet van een bereik van mogelijke herstellen punten worden behouden.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure Hiermee reduceert u de kans op pesterijen verlies van gegevens met geografische\-redundantie  
Back-ups van Azure-database zijn opgeslagen op geografische\-redundante Azure-opslag (AB\-GRS) en worden daarom geografische\-overtollige al dan niet standaard. Geografische\-redundante opslag uw gegevens worden gerepliceerd naar een secundaire gebied dat honderden mijl weg van het primaire regio. Uw gegevens worden in zowel primaire als secundaire regio's, drie keer elk, gerepliceerd naar afzonderlijke foutenstructuuranalyse domeinen en de upgrade domeinen. Dit zorgt ervoor dat uw gegevens blijvend via een programma worden zelfs in geval van een volledige regionale storing of noodgevallen dat een van de Azure regio's niet beschikbaar samenstelt is.

### <a name="stretchRPO"></a>Uitrekken Database Hiermee reduceert u de kans op pesterijen verlies van gegevens voor uw Azure-gegevens door tijdelijk gemigreerde rijen behouden
Nadat de Database uitrekken in aanmerking komend rijen uit SQL Server naar Azure migreert, behoudt de rijen in de tabel met het tijdelijk opslaan voor ten minste acht uur. Als u een back-up van uw Azure-database terugzet, gebruikmaakt uitrekken Database de rijen die zijn opgeslagen in de tabel tijdelijk opslaan van de SQL-Server en de Azure-databases afstemmen.

Nadat u een back-up van uw Azure gegevens terugzet, u moet uitvoeren van de opgeslagen procedure [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) om opnieuw verbinding maakt de uitrekken\-ingeschakeld van SQL Server-database met de externe Azure-database. Wanneer u **sys.sp_rda_reauthorize_db**uitvoert, Verenigd uitrekken Database automatisch de SQL-Server en de Azure-databases.

Als u wilt vergroten het aantal uren van gemigreerde gegevens dat uitrekken Database tijdelijk in het tijdelijk opslaan tabel behoudt, de opgeslagen procedure [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) uitvoeren en geef een aantal uren die groter zijn dan 8. Om te bepalen hoeveel gegevens moeten worden bewaard, moet u de volgende factoren overwegen:
-   De frequentie van automatische Azure met de back-ups (op minstens elke 8 uur).
-   De tijd die nodig na een probleem kunnen zien of het probleem en te bepalen herstellen van een back-up.
-   De duur van de bewerking voor Azure terugzetten.

> [AZURE.NOTE] De hoeveelheid gegevens die uitrekken Database tijdelijk in het tijdelijk opslaan tabel behoudt verhoogt, wordt de hoeveelheid ruimte op de SQL Server vereist.

Als u wilt controleren of het aantal uren dat uitrekken Database momenteel tijdelijk in de tabel tijdelijk opslaan behoudt, voert u de opgeslagen procedure [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx).

## <a name="see-also"></a>Zie ook

[Beheren en problemen met uitrekken Database](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Een back-Up en herstellen van SQL Server-Databases](https://msdn.microsoft.com/library/ms187048.aspx)
