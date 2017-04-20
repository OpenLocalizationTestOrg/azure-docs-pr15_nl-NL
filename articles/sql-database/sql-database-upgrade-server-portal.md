<properties
    pageTitle="Upgrade naar Azure SQL Database V12 met behulp van de Azure portal | Microsoft Azure"
    description="Wordt uitgelegd hoe u een upgrade uitvoeren naar Azure SQL Database V12 met inbegrip van het Web en bedrijfsdatabases upgraden en hoe u een V11: server zijn databases migreren rechtstreeks in een elastische database-toepassingen met behulp van de Azure portal bijwerken."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Upgrade naar Azure SQL Database V12 met behulp van de Azure portal


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL-Database V12 is de nieuwste versie, dus bestaande servers upgraden naar SQL-Database V12 wordt aanbevolen.
SQL-Database V12 heeft vele [voordelen ten opzichte van de vorige versie](sql-database-v12-whats-new.md) waaronder:

- Verbeterde compatibiliteit met SQL Server.
- Verbeterde premium prestaties en nieuwe prestaties.
- [Elastische database-toepassingen](sql-database-elastic-pool.md).

In dit artikel vindt u aanwijzingen voor het upgraden van bestaande V11: SQL-Database-servers en databases naar SQL-Database V12.

Tijdens het proces voor het upgraden naar V12 wordt u eventuele Web en bedrijfsdatabases bijgewerkt naar een nieuwe servicelaag, zodat de aanwijzingen voor het upgraden van Web en bedrijfsdatabases zijn opgenomen.

Daarnaast kan migreren naar een [groep elastische database](sql-database-elastic-pool.md) zijn kosten efficiënter dan een upgrade naar individuele prestatieniveaus (prijzen lagen) voor één databases. Databasebeheer van de vereenvoudigen van toepassingen ook omdat u hoeft slechts eenmaal voor het beheren van de prestatie-instellingen voor de groep in plaats van de prestaties van afzonderlijke databases afzonderlijk beheren. Als u databases op meerdere servers hebt kunt u ze worden verplaatst naar dezelfde server en profiteren van ze in een groep te plaatsen. U kunt eenvoudig een [auto-migreren van databases uit V11: servers rechtstreeks aan op elastische database van toepassingen via PowerShell](sql-database-upgrade-server-powershell.md). U kunt ook de portal V11: databases migreren naar een groep, maar in de portal hebt u al een server V12 maken van een groep. Aanwijzingen worden gegeven verderop in dit artikel voor het maken van de groep na de serverupgrade als u [databases die uit een groep kunnen profiteren](sql-database-elastic-pool-guidance.md)hebt.

Houd er rekening mee dat uw databases online blijft en gaat u verder met het werk voor de hele de upgrade uitvoert. Op het moment van de werkelijke overgang naar de nieuwe prestatieniveau tijdelijke weghalen verbindingen met de database kan kan optreden voor de duur van een zeer klein die meestal ongeveer 90 seconden maar zoveel 5 minuten. Als uw toepassing [tijdelijke foutenstructuuranalyse verwerking bij verbinding afsluitingen worden](sql-database-connectivity-issues.md) is het voldoende bescherming tegen decoratieve verbindingen aan het einde van de upgrade.

Een upgrade naar SQL-Database V12 kan niet ongedaan worden gemaakt. Na een upgrade kan niet de server naar V11: worden hersteld.

Na een upgrade naar V12, is [service laag aanbevelingen](sql-database-service-tier-advisor.md) en [Prestatieoverwegingen elastische van toepassingen](sql-database-elastic-pool-guidance.md) niet direct beschikbaar tot de service heeft tijd uw werkbelasting op de nieuwe server wordt berekend. V11: server aanbeveling geschiedenis geldt niet voor V12 servers, zodat deze blijft niet behouden.

## <a name="prepare-to-upgrade"></a>Upgrade voorbereiden

- **Alle Web en bedrijfsdatabases upgraden**: Zie [upgraden van alle Web en bedrijfsdatabases](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) sectie onder of raadpleegt u [beeldscherm en beheren van een elastische database-toepassingen (PowerShell)](sql-database-elastic-pool-manage-powershell.md).
- **Controleren en geografische herhaling schorten**: als uw Azure SQL-database is geconfigureerd voor geografische-replicatie moet u de huidige configuratie en de [geografische herhaling stoppen](sql-database-geo-replication-portal.md#remove-secondary-database)document. Nadat de upgrade is voltooid de configuratie van uw database voor geografische-replicatie.
- **Open deze poorten als er clients op een Azure-VM**: als uw clientprogramma verbinding maakt met SQL-Database V12 terwijl de klant wordt uitgevoerd op een Azure virtuele machine (VM), moet u bereikwaarden 11000-11999 en 14000-14999 op de VM openen. Zie [poorten voor V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)voor meer informatie.



## <a name="start-the-upgrade"></a>De upgrade starten

1. In de [portal van Azure](https://portal.azure.com/) bladeren naar de server die u wilt bijwerken door te **Bladeren**selecteren > **SQL-servers**en de v2.0-server die u wilt bijwerken te selecteren.
2. Selecteer de **Meest recente Update voor SQL-Database**en selecteer **deze server bijwerken**.

      ![upgraden van server][1]

3. Een upgrade van een server naar de meest recente Update van de SQL-Database is permanente en niet ongedaan worden gemaakt. Om te bevestigen de upgrade, typ de naam van de server en klik op **OK**.

## <a name="upgrade-all-web-and-business-databases"></a>Alle Web en bedrijfsdatabases upgraden

Als uw server geen Web of en grote ondernemingen-databases heeft, kunt u ze moet bijwerken. Tijdens het uitvoeren van een upgrade naar SQL-Database V12 verandert u alle Web en bedrijfsdatabases naar een nieuwe servicelaag.    

Om u te helpen u bij een upgrade uitvoert, wordt de SQL-Database-service aanbevolen een juiste service laag en prestaties niveau (prijzen laag) voor elke database. De service wordt aanbevolen de beste laag voor het uitvoeren van uw bestaande database werklast door het analyseren van de historische voor uw database.

3. Selecteer in het blad voor **deze server bijwerken** elke database te bekijken en selecteren als u wilt upgraden naar de aanbevolen prijzen trapsgewijs. U kunt altijd de beschikbare prijzen lagen bladeren en selecteer een die uw omgeving best past bij.


     ![databases][2]


7. Nadat u hebt geklikt van de voorgestelde laag wordt weergegeven met het blad **kiezen uw prijzen laag** waar u kunt een laag te selecteren en klik vervolgens op de knop **selecteren** om te wijzigen in die laag. Selecteer een nieuwe laag voor elke Web of en grote ondernemingen-database.

    Wat is belangrijk te weten is dat uw SQL-database niet is vergrendeld in elk niveau van de laag of prestaties voor specifieke services, dus als de vereisten van uw database wijziging u gemakkelijk tussen de beschikbare Servicelagen en de prestaties wijzigen kunt. Ja, Basic, Standard en Premium SQL-Databases worden gefactureerd door het uur en u hebt de mogelijkheid om te schalen dat elke database omhoog of omlaag 4 keer binnen een periode van 24 uur.

    ![aanbevelingen][6]


Nadat alle databases op de server in aanmerking komen bent u klaar voor de upgrade starten

## <a name="confirm-the-upgrade"></a>Bevestig de upgrade

3. Dat alle databases op de server in aanmerking komt voor de upgrade moet u **De servernaam TYPE** om te bevestigen dat u wilt de upgrade uitvoeren en klik vervolgens op **OK**.

    ![Controleer of de upgrade][3]


4. De upgrade wordt gestart en de voortgang in melding. Het upgradeproces is gestart. Afhankelijk van de details van uw specifieke databases upgraden naar V12 kan enige tijd duren. Tijdens deze periode alle databases op de server online blijven maar server en database management acties worden beperkt.

    ![upgrade wordt uitgevoerd][4]

    Op het moment van de werkelijke overgang naar de nieuwe prestaties kan niveau tijdelijke verbindingen met de database weghalen optreden voor de duur van een zeer klein (meestal gemeten in seconden). Als een toepassing tijdelijke foutenstructuuranalyse afhandeling (opnieuw logica) bij verbinding afsluitingen worden is voldoende bescherming tegen decoratieve verbindingen aan het einde van de upgrade heeft.

5. Nadat de upgrade bewerking is voltooid wordt **ingeschakeld**in het blad voor de **Meest recente Update** weergegeven.

    ![V12 ingeschakeld][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Uw databases verplaatsen naar een elastische database-toepassingen

Blader naar de server V12 en klik op **groep toevoegen**in de [portal van Azure](https://portal.azure.com/) .

- of -

Als u een bericht **Klik hier voor de aanbevolen elastische database van toepassingen voor deze server**ziet, klikt u erop om eenvoudig maken een resourcegroep die is geoptimaliseerd voor databases van de server. Zie voor meer informatie [Aandachtspunten voor de prijs en prestaties voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md).

![Groep toevoegen aan een server][7]

Volg de aanwijzingen in het artikel [een toepassingen elastische database maken](sql-database-elastic-pool.md) om te maken van uw groep.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Databases na een upgrade naar SQL-Database V12 controleren

>[AZURE.IMPORTANT] Upgrade uitvoeren naar de meest recente versie van SQL Server Management Studio (SSMS) om te profiteren van de nieuwe v12 mogelijkheden. [Downloaden SQL Server Management Studio] (https://msdn.microsoft.com/library/mt238290.aspx).

Na een upgrade uitvoert, kan de database om ervoor te zorgen toepassingen worden uitgevoerd op de gewenste prestaties te controleren en vervolgens optimaliseren instellingen zoals vereist.

Naast het controleren van afzonderlijke databases, kunt u Elastische database pools [Monitor, beheren, en de grootte van een elastische database-toepassingen met de portal van Azure](sql-database-elastic-pool-manage-portal.md) controleren of met [PowerShell](sql-database-elastic-pool-manage-powershell.md).


**Verbruik resourcegegevens:** Resourcegegevens verbruik is voor Basic, Standard en Premium databases beschikbaar via de [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV in de database. Deze DMV vindt u in de buurt van realtime verbruik resourcegegevens op 15 tweede granulatie het vorige uur bewerking. Het DTU percentage verbruik op een interval wordt berekend als het maximumpercentage verbruik van de processor, IO en log dimensies. Hier ziet u een query te berekenen van het gemiddelde DTU percentage verbruik via het laatste uur:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Aanvullende controlegegevens:

- [Azure SQL-Database prestaties richtlijnen voor één databases](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Aandachtspunten voor de prijs en prestaties voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md).
- [Cmdlets voor controle Azure SQL-Database dynamische management weergaven gebruiken](sql-database-monitoring-with-dmvs.md)




**Waarschuwingen:** 'Waarschuwingen instellen ' in de portal van Azure om u te waarschuwen wanneer het verbruik DTU voor de upgrade van database bepaalde hoog niveau nadert. Meldingen van de database kunnen worden ingesteld in Azure portal voor verschillende prestatiegegevens zoals DTU, CPU, IO en Log. Blader naar de database en selecteer **waarschuwingsregels** in het blad **Instellingen** .

U kunt bijvoorbeeld instellen van een e-mailwaarschuwing van "DTU Percentage" Als de gemiddelde waarde voor DTU percentage groter is dan 75% over de laatste 5 minuten. Raadpleegt u [meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) voor meer informatie over het configureren van waarschuwingen.





## <a name="next-steps"></a>Volgende stappen

- [Controleren op resourcegroep aanbevelingen en maken van een groep](sql-database-elastic-pool-create-portal.md).
- [Het serviceniveau laag en prestaties van uw database wijzigen](sql-database-scale-up.md).



## <a name="related-links"></a>Verwante koppelingen

- [Wat is er nieuw in SQL-Database V12](sql-database-v12-whats-new.md)
- [Plannen en voorbereiden voor het bijwerken naar SQL-Database V12](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
