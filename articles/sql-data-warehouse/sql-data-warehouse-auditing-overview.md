<properties
   pageTitle="Controle in SQL Azure datawarehouse | Microsoft Azure"
   description="Aan de slag met Azure SQL Data Warehouse controle"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Controle in SQL Azure datawarehouse

> [AZURE.SELECTOR]
- [Controle](sql-data-warehouse-auditing-overview.md)
- [Bedreiging detectie](sql-data-warehouse-security-threat-detection.md)

SQL Data Warehouse controle kunt u fouten in uw database om een controle Meld u aan uw account Azure Storage record. Controle, kunt u voor het behoud van regelgeving, inzichtelijk activiteit in een database maken en inzicht in de verschillen en afwijkingen die bedrijven bezwaren kunnen aangeven of verdachte problemen met de beveiliging. SQL Data Warehouse controle ook geïntegreerd met Microsoft Power BI voor inzoomen rapportage en analyse.

Controlehulpmiddelen inschakelen en naleving voor naleving worden vergemakkelijken, maar niet garanderen dat. Zie voor meer informatie over Azure-programma's die ondersteuning bieden voor naleving van standaarden, het <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Vertrouwenscentrum Azure</a>.

+ [Basisbeginselen van de database controleren]
+ [Controle voor uw database instellen]
+ [Controlelogboeken bijhouden en rapporten te analyseren]

##<a id="subheading-1"></a>Basisbeginselen van Azure SQL Data Warehouse Database controleren


Controle van SQL Data Warehouse-database, kunt u:

- **Behouden** een audittrail van de geselecteerde gebeurtenissen. U kunt categorieën van databaseacties moeten worden gecontroleerd definiëren.
- **Rapport** van de activiteit van de database. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met activiteiten en gebeurtenissen rapporteren.
- Rapporten **analyseren** . U vindt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends.

U kunt configureren voor de volgende gebeurteniscategorieën controleren:

**Gewone SQL** versus **SQL met parameters** waarvoor u de verzamelde controlelogboeken logboeken worden geclassificeerd als  

- **Toegang tot gegevens**
- **Wijzigingen in het schema (DDL)**
- **Wijzigingen aan de gegevens (DML)**
- **Accounts, rollen en machtigingen (Gegevensverbindingsbibliotheek)**
- **Opgeslagen Procedure**, **aanmelding** en **beheer van transacties**.

Controle van **geslaagde** en **mislukte** bewerkingen zijn voor elke categorie gebeurtenis afzonderlijk geconfigureerd.

Zie voor meer informatie over de activiteiten en gebeurtenissen gecontroleerd, de <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Indeling verwijzing naar controlelogboek (doc-bestand downloaden)</a>.

Controlelogboeken bijhouden worden opgeslagen in uw account Azure opslag. U kunt een bewaarperiode voor controle log definiëren.

Een controle beleid kan worden gedefinieerd voor een specifieke database of als een standaardbeleid voor de server. Er wordt een controle standaardbeleid voor server gelden voor alle databases op een server waarvoor geen een specifieke database controle beleid gedefinieerd.

Voordat u controle omhoog controle controleren als u een [' Downlevel Client '](sql-data-warehouse-auditing-downlevel-clients.md)gebruikt.


##<a id="subheading-2"></a>Controle voor uw database instellen

1. Start de <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>.

2. Ga naar het blad configuratie van de database van SQL Data Warehouse / SQL Server die u wilt controleren. Klik op de knop **Instellingen** bovenaan en klik vervolgens in het blad instelling en selecteer **controleren**.

    ![][1]

3. Klik in het blad controle configuratie, schakelt u eerst het selectievakje **Overnemen controle-instellingen van Server** . Hiermee kunt u de instellingen voor een bepaalde database opgeven.

    ![][2]

4. Vervolgens een controle door te klikken op de knop **op** inschakelen.

    ![][3]

5. Selecteer in het blad controle configuratie, **Opslag DETAILS** voor het openen van het controlelogboek logboeken opslag blad. Selecteer de opslaglocatie van Logboeken Azure opslag-account en de bewaarperiode. **Tip:** Gebruik hetzelfde opslag-account voor alle gecontroleerde databases naar optimaal gebruik van de vooraf geconfigureerde rapporten-sjablonen.

    ![][4]

6. Klik op de knop **OK** om het opslaan van de configuratie van de details van opslag.


7. Klik onder **LOGBOEKREGISTRATIE door GEBEURTENIS**op **GESLAAGDE** en **MISLUKTE** om alle gebeurtenissen of kies afzonderlijke gebeurteniscategorieën.


8. Als u controleren voor een database configureren wilt, moet u mogelijk de verbindingsreeks van de client om gegevens controle juist wordt vastgelegd wijzigen. Controleer het onderwerp van de [Server FULLY wijzigen in de verbindingstekenreeks](sql-data-warehouse-auditing-downlevel-clients.md) voor downlevel clientverbindingen.

9. Klik op **OK**.


##<a id="subheading-3">Controlelogboeken bijhouden en rapporten te analyseren</a>

Controlelogboeken bijhouden worden samengevoegd in een verzameling Store tabellen met een voorvoegsel **SQLDBAuditLogs** in het Azure opslag-mailaccount dat u hebt gekozen tijdens de installatie. U kunt een hulpmiddel zoals <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure opslag Explorer</a>gebruiken logboekbestanden weergeven.

Een rapportsjabloon vooraf geconfigureerde dashboard is beschikbaar als een <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadbare Excel-werkblad</a> kunt u snel analyseren logboekgegevens. De sjabloon wilt gebruiken op uw controlelogboeken bijhouden, moet u de Excel 2013 of hoger en Power Query die u kunt downloaden <a href="http://www.microsoft.com/download/details.aspx?id=39379">hier</a>.

De sjabloon fictieve voorbeeldgegevens erin heeft en u kunt Power Query instellen om te importeren van uw controlelogboek rechtstreeks vanuit uw Azure opslag-account.

Lees voor meer gedetailleerde informatie over het werken met de sjabloon kunt <a href="http://go.microsoft.com/fwlink/?LinkId=506731">How To (document downloaden)</a>.

![][5]


##<a id="subheading-4">Procedures voor gebruik in productie</a>
De beschrijving in deze sectie verwijst naar de volgende schermopnamen hierboven. <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> of <a href= "https://manage.windowsazure.com/" target="_bank">Klassieke Azure klassieke Portal</a> mag worden gebruikt.


##<a id="subheading-5"></a>Opslag sleutel opnieuw wordt gegenereerd

In productie bent u waarschijnlijk uw opslagruimte sleutels regelmatig vernieuwen. Vernieuwen wanneer uw sleutels moet u het beleid opnieuw opslaat. Het proces wordt als volgt:.


1. Schakel over de **Opslag toegangstoets** uit *primaire* *secundaire* en **Opslaan**in het controle configuratie blad (hierboven is beschreven in de set van de sectie controle).
![][4]
2. Ga naar de opslag configuratie blade en **opnieuw wilt genereren** de *Primaire sleutel van Access*.

3. Ga terug naar het blad controle configuratie, schakelt u de **Toegangstoets opslag** van *secundaire* om *primaire* en drukt u op **Opslaan**.

4. Ga terug naar de opslag UI en **genereren** de *Secundaire toegangstoets* (als voorbereiding van de volgende toetsen vernieuwingscyclus.

##<a id="subheading-6"></a>Automatisering
Er zijn verschillende PowerShell-cmdlets die kunt u controle configureren in Azure SQL-Database. Toegang tot de cmdlets voor controle moet PowerShell worden uitgevoerd in de modus van Azure resourcemanager.

> [AZURE.NOTE] De module [Azure resourcemanager](https://msdn.microsoft.com/library/dn654592.aspx) is momenteel in de proefversie. Deze mogelijk niet de capaciteiten van dezelfde als de Azure module bieden.

Wanneer u in de modus van Azure resourcemanager bent, voert `Get-Command *AzureSql*` voor een overzicht van de beschikbare cmdlets.


<!--Anchors-->
[Basisbeginselen van de database controleren]: #subheading-1
[Controle voor uw database instellen]: #subheading-2
[Controlelogboeken bijhouden en rapporten te analyseren]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
