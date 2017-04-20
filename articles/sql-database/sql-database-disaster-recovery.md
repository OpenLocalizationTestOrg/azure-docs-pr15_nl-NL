<properties
   pageTitle="SQL-Database herstel | Microsoft Azure"
   description="Informatie over het herstellen van een database van een storing regionale datacenter of mislukt met de Azure SQL Database actieve geografische-replicatie en mogelijkheden voor geografische herstellen."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Een Azure SQL-Database of failover in een secundair herstellen

Azure SQL-Database biedt de volgende mogelijkheden voor het herstellen van een storing:

- [Actieve geografische-herhaling](sql-database-geo-replication-overview.md)
- [Geografische herstellen](sql-database-recovery-using-backups.md#point-in-time-restore)

Meer informatie over bedrijfscontinuïteit scenario's voor bedrijven en de functies voor ondersteuning van deze scenario's, raadpleegt u [bedrijfscontinuïteit](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Voorbereiden voor de gebeurtenis van een storing

Voor succes met herstel naar een andere gegevensgebied actieve geografische-herhaling of geografische-redundante back-ups gebruikt, moet u voor het voorbereiden van een server in een andere Datacenter storing te worden de nieuwe primaire server dat u moet zich voordoen, evenals de stappen beschreven en een vloeiende herstel getest goed hebt gedefinieerd. Deze voorbereidende stappen nodig zijn:

- De logische server in een andere regio om de nieuwe primaire server vertrouwd te identificeren. Met actieve geografische-herhaling, is dit ten minste één en wellicht een van de secundaire servers. Voor geografische-herstellen is dit in het algemeen een server in de [gepaarde regio](../best-practices-availability-paired-regions.md) voor de regio waarin de database zich bevindt.
- Identificeren en desgewenst definiëren, de serverniveau firewallregels op voor gebruikers toegang hebben tot de nieuwe primaire database nodig.
- Hiermee bepaalt u hoe u gaat om gebruikers te leiden naar de nieuwe primaire server, zoals door te wijzigen van verbindingstekenreeksen met de of wijzigen van DNS entries.
- Identificeren en desgewenst maken, de aanmeldingen waaraan moeten aanwezig zijn in de hoofddatabase op de nieuwe primaire server, en er zeker van deze aanmeldingen juiste machtigingen in de hoofddatabase, indien van toepassing. Zie [SQL-Database beveiliging na herstel](sql-database-geo-replication-security-config.md) voor meer informatie
- Identificeer waarschuwingsregels die moeten worden bijgewerkt als u wilt toewijzen aan de nieuwe primaire database.
- De controle configuratie op de hoofddatabase van het huidige document
- Uitvoeren op een [herstel inzoomen](sql-database-disaster-recovery-drills.md). U kunt deze de vorm van een storing voor geografische herstellen, verwijderen of de naam van de brondatabase als u de toepassing connectivity mislukt wijzigen. Deze de vorm van een storing aan voor Active geografische-replicatie, kunt u de webtoepassing of VM die zijn verbonden met de database of failover de database om te leiden tot toepassingsfouten connectity uitschakelen.

## <a name="when-to-initiate-recovery"></a>Wanneer u het herstelproces te starten

De herstelbewerking van invloed op de toepassing. Dit is vereist voor het wijzigen van de SQL-verbindingsreeks of omleiding met DNS en kan leiden tot permanent gegevensverlies. Daarom moet dit doen alleen als de storing is waarschijnlijk het langer dan van uw toepassing herstel tijd doelstelling. Wanneer de toepassing wordt geïmplementeerd op productie moet u uitvoeren regelmatige controle van de toepassing gezondheid en gebruik van de volgende gegevenspunten om te bevestigen dat het herstelproces is nodig:

1.  Permanente connectivity mislukt uit de toepassingslaag met de database.
2.  De portal van Azure ziet u een melding over een incident in het gebied met globaal impact.
3.  De Azure SQL Database-server is gemarkeerd als niet beschikbaar is weergegeven.

Afhankelijk van uw toepassing tolerantie downtime en mogelijk bedrijven aansprakelijkheid kunt u rekening houden met de volgende herstelopties.

Gebruik de [Herstelbare Database ophalen](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) om de meest recente geografische gerepliceerd herstellen komma.

## <a name="wait-for-service-recovery"></a>Wachten op een service herstel

De werklast Azure teams naar eer en geweten om terug te zetten beschikbare service zoals snel mogelijk, maar afhankelijk van de hoofdsite ertoe leiden dat deze kan duren, uren of dagen.  Als uw toepassing aanzienlijk downtime mogelijk zonder kunt u gewoon wacht totdat het herstelproces is voltooid. In dit geval is geen actie ondernemen vereist. De huidige status van service kunt u zien op onze [Dashboard voor servicestatus van Azure-Service](https://azure.microsoft.com/status/). Nadat het herstelproces is van het gebied worden van de toepassing beschikbaar hersteld.

## <a name="failover-to-geo-replicated-secondary-database"></a>Failover naar secundaire database geografische gerepliceerd

Als de downtime van uw toepassing in business aansprakelijkheid resulteren kan moet u geografische-gerepliceerde database (s) in uw toepassing. De toepassing beschikbaar is in een ander gebied in het geval van een storing snel herstellen wordt het ingeschakeld. Informatie over het [configureren van geografische-herhaling](sql-database-geo-replication-portal.md).

Als u wilt herstellen van de beschikbaarheid van de database (s) die u moet de overname aan de secundaire geografische gerepliceerd met een van de ondersteunde methoden initiëren.

Gebruik een van de volgende handleidingen foutherstel met een secundaire geografische gerepliceerd-database:

- [Failover naar een secundair geografische gerepliceerd met behulp van Azure-Portal](sql-database-geo-replication-portal.md)
- [Failover naar een secundair geografische gerepliceerd via PowerShell](sql-database-geo-replication-powershell.md)
- [Failover naar een secundair geografische gerepliceerd met T-SQL](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Geografische herstellen met herstellen

Als van uw toepassing downtime niet in business aansprakelijkheid resulteert kunt u geografische herstellen als een methode voor het herstellen van uw databases toepassing. Er een kopie van de database uit de meest recente geografische-redundante back-up gemaakt.

Gebruik een van de volgende handleidingen geografische terugzetten een database naar een nieuwe regio:

- [Een database naar een nieuwe gebied met behulp van Azure Portal geografische-herstellen](sql-database-geo-restore-portal.md)
- [Een database naar een nieuwe gebied via PowerShell geografische-herstellen](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>De database na herstel configureren

Als u geografische herhaling foutovername of geografische herstellen herstellen uit een storing gebruikt, moet u ervoor zorgen dat de verbinding met de nieuwe databases correct is geconfigureerd, zodat de functie normale toepassing kunt hervatten. Dit is een controlelijst met taken voor het voorbereiden van uw productie herstelde database.

### <a name="update-connection-strings"></a>Verbindingstekenreeksen met de bijwerken

Omdat de herstelde database bevindt zich in een andere server, moet u de verbindingsreeks van de toepassing te laten verwijzen naar die server bijwerken.

Zie de juiste ontwikkeling taal voor uw [Verbindingsbibliotheek](sql-database-libraries.md)voor meer informatie over het wijzigen van verbindingstekenreeksen met de.

### <a name="configure-firewall-rules"></a>De firewallregels configureren

U nodig hebt om ervoor te zorgen dat de firewallregels die is geconfigureerd op de server en klik op de database overeenkomen met de waarden voor de primaire server en primaire database zijn geconfigureerd. Zie voor meer informatie [hoe: firewallinstellingen configureren (Azure SQL-Database)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Aanmeldingen configureren en gebruikers Database

U nodig hebt om ervoor te zorgen dat alle aanmeldingen gebruikt door de toepassing op de server die als de herstelde database host fungeert bestaan. Zie [Configuratie van de beveiliging voor geografische-replicatie](sql-database-geo-replication-security-config.md)voor meer informatie.

>[AZURE.NOTE] U moet configureren en testen van uw server firewallregels en aanmeldingen (en hun machtigingen) tijdens een noodgevallen herstel meer details. Deze objecten serverniveau en hun configuratie mogelijk niet beschikbaar tijdens de storing.

### <a name="setup-telemetry-alerts"></a>Telemetrielogboek waarschuwingen instellen

U nodig hebt om ervoor te zorgen dat uw bestaande waarschuwing regelinstellingen om toe te wijzen aan de herstelde database en de andere server worden bijgewerkt.

Zie voor meer informatie over waarschuwingsregels database, [Ontvangen een melding voor meldingen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) en [Servicestatus bijhouden](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Controle inschakelen

Als controle is vereist voor toegang tot uw database, moet u controle inschakelen nadat het herstelproces is database. Een goede indicator dat controle vereist is is dat clienttoepassingen beveiligde verbindingstekenreeksen in een patroon van gebruiken *. database.secure.windows.net. Zie [aan de slag met het controleren van de SQL-database](sql-database-auditing-get-started.md)voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Azure SQL-Database wordt automatisch back-ups, raadpleegt u [SQL-Database automatische back-ups](sql-database-automated-backups.md)
- Zie voor meer informatie over bedrijfscontinuïteit ontwerp- en herstelbestanden scenario's voor bedrijven, [bedrijfscontinuïteit scenario 's](sql-database-business-continuity.md)
- Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een van de service gestart back-ups van een database terugzetten](sql-database-recovery-using-backups.md)
