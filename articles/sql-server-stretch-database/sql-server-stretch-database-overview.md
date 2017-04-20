<properties
    pageTitle="Overzicht van de Database uitrekken | Microsoft Azure"
    description="Leer hoe uitrekken Database migreert uw koudwatersystemen gegevens transparant en veilig met de cloud Microsoft Azure."
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
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Overzicht van de Database uitrekken

Uitrekken Database migreert uw koudwatersystemen gegevens transparant en veilig met de cloud Microsoft Azure.

Als u wilt alleen aan de slag met uitrekken Database direct af, raadpleegt u [aan de slag door de Database inschakelen voor uitrekken Wizard uit te voeren](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Wat zijn de voordelen van uitrekken Database?
Uitrekken Database biedt de volgende voordelen:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Kosten biedt\-effectieve beschikbaarheid voor koudwatersystemen gegevens
Spreid warm- en koudwatersystemen transacties gegevens dynamisch uit SQL Server Microsoft Azure met uitrekken van SQL Server-Database. In tegenstelling tot normale koudwatersystemen gegevensopslag is uw gegevens altijd online en beschikbaar zijn om query's. Meer gegevens bewaarbeleid tijdlijnen kan worden verstrekt zonder te verbreken van de bank voor grote tabellen zoals ordergeschiedenis klant. Profiteren van de lage kosten van Azure in plaats van schaalbaarheid dure, op\-bedrijfsruimten opslag. U kiest de prijzen laag en instellingen configureren in de Portal Azure te onderhouden controle over kosten. Schaal omhoog of omlaag naar wens. Ga naar de pagina [SQL Server uitrekken Database prijzen](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) voor meer informatie.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Wijzigingen in query's of toepassingen vereisen niet
Toegang tot uw SQL Server-gegevens naadloos ongeacht of het nu gaat op\-bedrijfsruimten of uitgerekt in de cloud.  U instellen het beleid dat bepaalt waar gegevens worden opgeslagen en SQL Server omgaat met de verplaatsing van de gegevens op de achtergrond. De hele tabel is altijd online en eigenschappenfilters. En, uitrekken Database geen wijzigingen van bestaande query's of toepassingen nodig: de locatie van de gegevens volledig transparant voor de toepassing is.

### <a name="streamlines-on-premises-data-maintenance"></a>Gestroomlijnd op\-bedrijfsruimten gegevensonderhoud
Beperken op\-bedrijfsruimten onderhoud en opslag van uw gegevens. Back-ups voor uw op\-lokale gegevens sneller worden uitgevoerd en einddatum in het onderhoudsvenster. Back-ups voor de cloud-gedeelte van uw gegevens automatisch wordt uitgevoerd. Uw op\-lokale opslagbehoeften aanzienlijk zijn verminderd. Azure opslaggebied kan zich bevinden 80% goedkoper is dan het toevoegen van naar aan\-bedrijfsruimten SSD.

### <a name="keeps-your-data-secure-even-during-migration"></a>Uw gegevens houdt veilig zelfs tijdens de migratie
Als u uw belangrijkste toepassingen veilig met de cloud uitrekken te rekenen op gemoedsrust. SQL-Server altijd versleuteld bevat versleuteling voor uw gegevens in beweging. Rij niveau beveiliging (RLS) en andere geavanceerde SQL Server-beveiligingsvoorzieningen werken ook met de Database uitrekken om uw gegevens te beveiligen.

## <a name="what-does-stretch-database-do"></a>Wat doet uitrekken Database?
Nadat u de Database uitrekken voor een SQL Server-instantie, een database en ten minste één tabel inschakelt, begint uitrekken Database stilte om te migreren van uw koudwatersystemen gegevens naar Azure.

-   Als u koudwatersystemen gegevens in een aparte tabel opslaat, kunt u de hele tabel migreren.

-   Als uw tabel warm- en koudwatersystemen gegevens bevat, kunt u een filterfunctie om de rijen om te migreren te selecteren.

**U hoeft niet te wijzigen van bestaande query's en client-apps.** U gaat u verder met het naadloze toegang hebben tot lokale en externe gegevens, ook tijdens de gegevensmigratie. Er is een kleine hoeveelheid latentie voor externe query's, maar u alleen dit latentie optreden wanneer u een query uitvoeren op de koudwatersystemen gegevens.

**Dat uitrekken Database zorgt ervoor dat geen gegevens verloren** als er een fout optreedt tijdens de migratie. Er wordt ook opnieuw logica worden afgehandeld verbindingsproblemen die tijdens de migratie optreden kunnen. Een weergave dynamische beheer biedt de status van de migratie.

**U kunt gegevensmigratie onderbreken** problemen op de lokale server of u kunt de beschikbare netwerkbandbreedte optimaliseren.

![Overzicht van de database uitrekken][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Uitrekken Database is voor u?
Als u de volgende beweringen maakt kunt, kunt uitrekken Database om te voldoen aan uw behoeften en uw problemen oplossen.

|Als u een maker beslissing|Als u een DBA|
|------------------------------|-------------------|
|Ik heb transacties om gegevens te houden voor een lange tijd.|De grootte van Mijn tabellen is ontvangen uit besturingselement.|
|Ik heb soms om query's in de koudwatersystemen gegevens.|Mijn gebruikers zegt dat ze toegang tot koudwatersystemen gegevens wilt, maar ze deze alleen zelden gebruiken.|
|Ik heb-apps, waaronder oudere apps, die ik niet wil bijwerken.|Ik moet houden kopen en meer opslagruimte wilt toevoegen.|
|Ik wil een manier om op te slaan geld op opslag zoeken.|Ik kan geen back-up maken of deze grote tabellen binnen de SLA herstellen.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Welk soort databases en tabellen zijn kandidaten voor uitrekken Database?
Spreid Database doelen transacties databases met grote hoeveelheden koudwatersystemen gegevens, meestal die zijn opgeslagen in een klein aantal tabellen. Deze tabellen mogelijk meer dan een miljard rijen bevatten.

Als u de functie tijdelijke tabel van SQL Server-2016 gebruikt, uitrekken Database gebruiken om te migreren van een bestand of een deel van het bijbehorende geschiedenistabel kosten\-effectieve opslagruimte in Azure wordt aangegeven. Zie voor meer informatie, [Bewaarbeleid van historische gegevens beheren in systeem versienummer tijdelijke tabellen](https://msdn.microsoft.com/library/mt637341.aspx).

Gebruik uitrekken Database Advisor, een functie van SQL Server 2016 Upgrade Advisor, databases en tabellen voor uitrekken Database identificeren. Zie [identificeren databases en tabellen voor uitrekken Database](sql-server-stretch-database-identify-databases.md)voor meer informatie. Meer informatie over problemen met de blokkeren, Zie [beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Uitproberen uitrekken Database
**Test station uitrekken Database met de voorbeelddatabase AdventureWorks.** Als u de voorbeelddatabase AdventureWorks, download ten minste het databasebestand en het bestand voorbeelden en scripts vanuit [hier](https://www.microsoft.com/download/details.aspx?id=49502). Nadat u de voorbeelddatabase op een exemplaar van SQL Server-2016 terugzetten, pak het bestand en opent u het bestand uitrekken DB uit de map uitrekken DB. Voer de scripts in dit bestand om te controleren van de ruimte gebruikt door uw gegevens voordat en nadat u hebt ingeschakeld uitrekken Database, voor het bijhouden van de voortgang van de gegevensmigratie en om te bevestigen dat u kunt doorgaan met query van bestaande gegevens en voegt u nieuwe gegevens zowel tijdens en na de gegevensmigratie.

## <a name="next-step"></a>Volgende stap
**Welke databases en tabellen die kandidaten voor uitrekken Database zijn.** SQL Server 2016 Upgrade Advisor downloaden en installeren van de uitrekken Database Advisor om databases en tabellen die kandidaten voor uitrekken Database zijn te identificeren. Uitrekken Database Advisor kunt u ook blokkeren problemen identificeren. Zie [identificeren databases en tabellen voor uitrekken Database](sql-server-stretch-database-identify-databases.md)voor meer informatie.

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
