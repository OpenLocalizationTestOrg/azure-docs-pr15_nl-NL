<properties
    pageTitle="De upgrade naar SQL-Database V12 plannen | Microsoft Azure"
    description="Worden de voorbereidingen en beperkingen voor het upgraden naar versie V12 van Azure SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Plannen en voorbereiden voor het bijwerken naar SQL-Database V12


In dit onderwerp worden de planning en de voorbereidingen die u moet uitvoeren om de upgrade van uw Azure SQL-databases van versie V11: naar V12.


Een nieuwe [Azure-Portal](https://portal.azure.com/) is beschikbaar voor de upgrade naar V12 ondersteunen.


De volgende tabel bevat andere Help-onderwerpen voor V12.


| Titel en de koppeling | Beschrijving van inhoud |
| :--- | :--- |
| [Wat is er nieuw in SQL-Database V12](sql-database-v12-whats-new.md) | Beschrijving van de details van hoe V12 Azure SQL-Database dichter naar volledige overeenkomst met Microsoft SQL Server brengt. |
| [Een database maken in SQL-Database V12](sql-database-get-started.md) | Wordt beschreven hoe u een nieuwe Azure SQL-database versie V12 kunt maken. Hierin worden verschillende opties voorbij alleen een lege database. |


## <a name="plan-ahead"></a>Vooruit te plannen


De volgende subsecties beschrijven het onderwijs en besluitvorming die u houden moet voordat u een acties richting van een upgrade van uw Azure SQL-database naar V12 uitvoeren.

### <a name="version-clarification"></a>Uitleg van de versie


In dit document heeft betrekking op de upgrade van Microsoft Azure SQL-Database van versie V11: naar V12. Meer formeel de versienummers zijn dicht bij de volgende twee waarden, volgens de Transact-SQL-instructie **Selecteer @@version; ** :


- 12.0.2000.8 *(of een hogere, bits V12)*
- 11.0.9228.18 *(V11:)*
 - Soms is V11: ook genoemd SAWA v2.

### <a name="service-tier-planning"></a>Service-laag plannen


Beginnen met V12, ondersteunt Azure SQL-Database alleen de Servicelagen met de naam Basic, Standard en Premium. De service Web en bedrijven laag wordt niet ondersteund op V12. Daarom als u van plan bent de upgrade van uw Azure SQL-database die momenteel is in de service-laag Web of en grote ondernemingen, moet u bepalen wat de nieuwe servicelaag moet zijn.


Zie voor gedetailleerde informatie over de service Basic, Standard en Premium lagen:

- [SQL-Database Servicelagen](sql-database-service-tiers.md)
- [SQL-Database Web/bedrijfsdatabases upgraden naar nieuwe Servicelagen](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>De geografische-replicatie-configuratie controleren


Als uw Azure SQL-database is geconfigureerd voor geografische-replicatie, moet u de huidige configuratie document en geografische-replicatie stoppen voordat u de acties aan de upgrade voorbereiden begint. Nadat de upgrade is voltooid moet u de database opnieuw configureren voor geografische-replicatie.


De strategie is de bron behouden en testen op een kopie van de database.


## <a name="preparation-actions"></a>Voorbereiding van acties


Als u klaar bent uw planning, kunt u de actie stappen in de volgende subsecties voorbereiden op het einde van de upgrade kunt uitvoeren.


Gedetailleerde beschrijvingen van de laatste upgrade fase zijn beschikbaar in de Help-onderwerpen die zijn gekoppeld aan aan de bovenkant van dit Help-onderwerp.


### <a name="service-tier-actions"></a>Service laag acties


De prijzen van laag Web- en -service wordt niet ondersteund op V12.


Als uw V11: Azure SQL-database een database Web of en grote ondernemingen is, wordt het upgradeproces biedt om te schakelen van de database naar een ondersteunde laag. De upgrade wordt aanbevolen een laag die voldoet aan de geschiedenis van de werklast van uw database. U kunt echter elke ondersteunde laag die u tevreden bent.


U kunt stapsgewijze instructies tijdens de upgrade verkleinen door over te schakelen van uw database V11: van de Web-en-Business-laag af voordat u de upgrade starten. U kunt dit doen met behulp van de nieuwe [Azure-Portal](https://portal.azure.com/).


Als u niet zeker welke servicelaag om te schakelen weet naar, is het S2 niveau van de standaard laag mogelijk een functionele eerste keuze. Elke lagere laag heeft minder bronnen dan de Web- en laag had.


### <a name="suspend-geo-replication-during-upgrade"></a>Geografische herhaling schorten tijdens de upgrade


De upgrade naar V12 kan worden uitgevoerd als geografische herhaling actief op uw database is. U moet eerst uw database als u wilt stoppen met het gebruik van geografische herhaling opnieuw te configureren.


Nadat de upgrade is voltooid, kunt u de database opnieuw gebruik geografische herhaling kunt configureren.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Open poorten op een VM Azure voor clientconnectiviteit


Als uw clientprogramma verbinding maakt met SQL-Database V12 terwijl de klant wordt uitgevoerd op een Azure virtuele machine (VM), moet u de volgende bereikwaarden op de VM openen:

- 11000-11999
- 14000-14999


Klik [hier](sql-database-develop-direct-route-ports-adonet-v12.md) voor meer informatie over de poorten voor SQL-Database V12. Verbeterde prestaties in SQL-Database V12 de poorten nodig.


##<a id="limitations"></a>Beperkingen tijdens en na een upgrade naar V12


### <a name="portals-for-v12"></a>Portals voor V12


Er zijn drie portals voor Azure en elk verschillende mogelijkheden aangaande SQL-Database V12 heeft.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Deze Azure-Portal is nieuw en bevindt zich nog in de preview-status. Deze portal nog niet helemaal bij volledige algemene beschikbaarheid (GA). Deze portal:
 - Beheer uw V12 server en database.
 - De database V11: kan worden upgraden naar V12.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Deze klassieke Azure-Portal mogelijk uiteindelijk geleidelijk. Deze portal:
 - Beheer uw V12 server en database.
 - Kan *geen* upgrade uw V11: database naar V12.


- (http://*servernaam*. database.windows.net)<br/>Azure SQL Database klassieke Portal:
 - Kan*geen* V12 servers beheren.


We raden u verbinding maakt met uw Azure SQL-databases met Visual Studio 2015 (VS2015). VS2015 kan worden gebruikt voor taken zoals het volgende:


- Naar een Transact-SQL-instructie uitvoert.
- Bij het ontwerpen van een schema.
- Het ontwikkelen van een database, online of offline.


Of, in plaats daarvan gratis kunt u downloaden [Visual Studio-Community-2015](https://www.visualstudio.com/vs/community/), dat wil een volledige versie van VS2015 zeggen.


In de oudere Azure klassieke-Portal op de databasepagina, kunt u **in Visual Studio is geopend** om te starten VS2015 op uw computer voor verbinding met uw Azure SQL-Database.


U kunt voor een andere plaats, SQL Server Management Studio (SSMS) 2014 gebruiken met [CU6](http://support.microsoft.com/kb/3031047/) verbinding maken met Azure SQL-Database. Er zijn meer gegevens op dit blogbericht:<br/>[Client gereedschap updates voor Azure SQL-Database](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Beperking *tijdens de* upgrade naar V12


De database V11: blijft beschikbaar voor toegang tot gegevens tijdens de upgrade naar V12. Er zijn nog enkele beperkingen u rekening moet houden.


| Beperking | Beschrijving |
| :--- | :--- |
| Duur van een upgrade | De duur van een upgrade, is afhankelijk van de grootte, de edition en het aantal databases in de server. Het upgradeproces kan worden uitgevoerd voor uren aan dagen voor servers met name voor servers waarop databases:<br/><br/>*Groter zijn dan 50 GB, of<br/> * In een niet-premium servicelaag<br/><br/>Het maken van nieuwe databases op de server tijdens de upgrade kunt ook de upgrade duur verlengen. |
| Geen geografische-replicatie | Geografische-replicatie wordt niet ondersteund op een server V12 die momenteel is betrokken bij een upgrade van V11:. |
| Database is kort niet beschikbaar in de laatste fase van een upgrade naar V12 | De databases die deel uitmaken van uw server V11: blijven beschikbaar tijdens de upgrade. De verbinding met de server en databases is echter kort niet beschikbaar in de laatste fase, waarop de schakeloptie via uit V11: aan de klaar V12 begint.<br/><br/>De schakeloptie periode kan variëren van 40 seconden tot 5 minuten. Voor de meeste servers, schakelen over naar verwachting voltooid binnen 90 seconden. Hiermee verhoogt u schakeloptie na verloop van tijd voor servers met een groot aantal databases, of wanneer de databases dik schrijven werkbelasting hebben. |


### <a name="limitation-after-upgrade-to-v12"></a>Beperking *nadat* upgraden naar V12


| Beperking | Beschrijving |
| :--- | :--- |
| Kan niet teruggaan naar V11: | Na een upgrade ter plaatse, niet kan het resultaat worden teruggezet of ongedaan gemaakt. |
| Web of en grote ondernemingen laag | Nadat de upgrade wordt gestart, de server voor de nieuwe V12-database kunnen niet meer herkend of de service Web of en grote ondernemingen laag accepteren. |



### <a name="export-and-import-after-upgrade-to-v12"></a>*Nadat* de upgrade naar V12 importeren en exporteren


U kunt exporteren of importeren van een database V12 met behulp van de [Azure-Portal](https://portal.azure.com/). Of u kunt exporteren of importeren met een van de volgende hulpmiddelen:


- SQL Server Management Studio (SSMS)
- Visual Studio-2015
- Gegevens laag Application Framework (DacFx)


Echter om de services, moet u eerst hebt of installeer de meest recente updates om ervoor te zorgen dat ze ondersteuning voor de nieuwe functies voor V12:


- [Cumulatieve Update 6 voor SQL Server Management Studio 2014](http://support2.microsoft.com/kb/3031047)
- [De Update februari 2015 voor SQL Server-Database gereedschap in Visual Studio 2013](https://msdn.microsoft.com/data/hh297027)
- [Februari 2015 gegevens laag Application Framework (DacFx) voor Azure SQL Database V12](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] De voorgaande hulpmiddel koppelingen zijn bijgewerkt op of na 2 maart 2015. Het is raadzaam deze nieuwere updates van deze hulpprogramma's te gebruiken.


#### <a name="automated-export"></a>Geautomatiseerde exporteren


Geautomatiseerde exporteren blijft beschikbaar als preview.  Geen client hulpmiddel-updates zijn vereist wanneer u automatische exporteren.  Desgewenst kunt u het bestand en importeren in een on-premises-server nodig de bovenstaande tooling-updates zijn geïmporteerd.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>V12 van een verwijderde V11: database herstellen

De volgende scenario wordt uitgelegd dat een verwijderde V11: Azure SQL-database naar een V12 Azure SQL Database-server kan worden hersteld.

1. Stel dat u hebt een V11: Azure SQL Database-server. <br/> Op de server hebt u een database in de verouderde Web-en-Business-servicelaag.
2. U verwijderen de database.
3. U kunt de server bijwerken naar V12.
4. U kunt naast de database terugzetten naar de server. <br/> De database wordt daarmee ook bijgewerkt naar V12, op het niveau S0 van de laag Standard service.
5. U kunt de database overschakelen naar elke laag ondersteunde service als S0 niet uw voorkeur is.


### <a name="powershell-cmdlets"></a>PowerShell-cmdlets


PowerShell-cmdlets zijn beschikbaar voor starten, stoppen of een upgrade naar Azure SQL Database V12 uit V11: of een andere versie van de pre-V12 controleren.

- [Upgrade naar SQL-Database V12 via PowerShell](sql-database-upgrade-server-powershell.md)

Voor de documentatie over deze PowerShell-cmdlets, raadpleegt u:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Stop-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


De Stop-cmdlet betekent annuleren, niet onderbreken. Er is geen manier om een upgrade, dan opnieuw te starten vanaf het begin hervatten. De Stop-cmdlet opgeschoond en versies van alle gewenste resources.


## <a name="failure-resolution"></a>Fout bij resolutie


Als de upgrade om een oneven reden mislukt, blijft de database V11: actief en beschikbaar zijn als normale.


## <a name="related-links"></a>Verwante koppelingen


- Microsoft Azure- [functies](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
