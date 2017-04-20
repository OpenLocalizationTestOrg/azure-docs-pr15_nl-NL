<properties 
   pageTitle="SQL-Database noodgevallen herstel boren | Microsoft Azure" 
   description="Meer informatie vinden en aanbevolen procedures voor het gebruik van Azure SQL-Database om uit te voeren noodgevallen herstel oefeningen die helpt behouden de opdracht kritieke bedrijfstoepassingen robuuste fouten en bijvoorbeeld." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Uitvoering van noodgevallen herstel meer details

Het wordt aanbevolen dat validatie van de gereedheid van de toepassing voor de werkstroom voor het herstelproces is geregeld wordt uitgevoerd. Bevestigt het gedrag van de toepassing en de gevolgen van gegevensverlies en/of de verstoring houdt in dat failover is een goede gewoonte technische. Het is ook een vereiste door de meeste industrienormen als onderdeel van de bedrijven bedrijfscontinuïteit-certificering.

Uitvoering van een noodgevallen herstel detailanalyse bestaat uit:

- Simulating gegevens laag storing
- Herstellen 
- Valideer de integriteit post-toepassing herstellen

Afhankelijk van hoe u [uw toepassing voor bedrijfscontinuïteit ontworpen](sql-database-business-continuity.md), de werkstroom voor het uitvoeren van de meer details kan variëren. Hieronder beschreven de aanbevolen procedures voor het uitvoeren van een noodgevallen herstel meer details in de context van Azure SQL-Database. 

##<a name="geo-restore"></a>Geografische herstellen

Als u wilt voorkomen dat de potentieel gegevensverlies wanneer een noodgevallen herstel detailanalyse houdt, wordt u aangeraden uitvoering van het gebruik van een testomgeving door een kopie van de productieomgeving maken en deze voor de verificatie van de toepassing failover werkstroom inzoomen.
 
####<a name="outage-simulation"></a>Storing simulatie

U kunt deze de vorm van de storing verwijderen of de naam van de brondatabase wijzigen. Hierdoor wordt de toepassing connectivity is mislukt. 

####<a name="recovery"></a>Herstel

- De geografische-terugzetten van de database naar een andere server als beschreven [hier](sql-database-disaster-recovery.md)uitvoeren. 
- Wijzig de configuratie van de toepassing als u wilt verbinden met de herstelde database (s) en volgt u de handleiding voor [een database na herstel configureren](sql-database-disaster-recovery.md) om het herstelproces is voltooid.

####<a name="validation"></a>Gegevensvalidatie

- Hiermee voert u de analyse door te verifiëren van de toepassing integriteit bericht herstellen (dat wil zeggen verbindingstekenreeksen met de, aanmeldingen, basisfunctionaliteit testen of andere validatie deel van standaardtoepassing ondertekeningen procedures).

##<a name="geo-replication"></a>Geografische-herhaling

Voor een database die is beveiligd met geografische-replicatie wordt de oefening voor meer details in gebruikmaakt van geplande failover naar de secundaire database. De geplande overname zorgt ervoor dat de primaire en de secundaire databases synchroon blijven wanneer de rollen worden veranderd. In tegenstelling tot de niet-geplande overname resulteert deze bewerking niet in verlies van gegevens, zodat de analyse kan worden uitgevoerd in de productieomgeving. 

####<a name="outage-simulation"></a>Storing simulatie

Als u wilt de storing simuleren kunt u de webtoepassing of VM die zijn verbonden met de database uitschakelen. Resulteert dit in de fouten connectivity voor de webclients.

####<a name="recovery"></a>Herstel

- Zorg ervoor dat de configuratie van de toepassing in de regio DR verwijst naar de voormalige secundaire wordt volledig toegankelijk nieuwe primair namelijk. 
- [Geplande failover](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) voordat u de secundaire database een nieuwe primaire uitvoeren
- Volg de handleiding voor [een database na herstel configureren](sql-database-disaster-recovery.md) om het herstelproces is voltooid.

####<a name="validation"></a>Gegevensvalidatie

- Hiermee voert u de analyse door te verifiëren van de toepassing integriteit bericht herstellen (dat wil zeggen verbindingstekenreeksen met de, aanmeldingen, basisfunctionaliteit testen of andere validatie deel van standaardtoepassing ondertekeningen procedures).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over zakelijke bedrijfscontinuïteit scenario's, Zie [bedrijfscontinuïteit scenario 's](sql-database-business-continuity.md)
- Meer informatie over Azure SQL-Database wordt automatisch back-ups, raadpleegt u [SQL-Database automatische back-ups](sql-database-automated-backups.md)
- Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een van de service gestart back-ups van een database terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over sneller herstelopties, [Actief-geografische-replicatie](sql-database-geo-replication-overview.md)  
