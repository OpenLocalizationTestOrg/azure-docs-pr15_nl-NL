<properties 
   pageTitle="Azure SQL Database Advisor met behulp van de Azure portal | Microsoft Azure" 
   description="U kunt de Advisor Azure SQL-Database in de portal van Azure gebruiken om te controleren en implementeren van aanbevelingen voor uw bestaande SQL-Databases die u kunt de huidige queryprestaties verbeteren." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>SQL-Database Advisor met behulp van de Azure portal

> [AZURE.SELECTOR]
- [SQL-Database Advisor overzicht](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

U kunt de Advisor Azure SQL-Database in de portal van Azure gebruiken om te controleren en implementeren van aanbevelingen voor uw bestaande SQL-Databases die u kunt de huidige queryprestaties verbeteren.

## <a name="viewing-recommendations"></a>Aanbevelingen weergeven

De pagina aanbevelingen is waar u de bovenste aanbevelingen op basis van hun gevolgen prestaties te verbeteren bekijken. U kunt ook de status van de historische bewerkingen bekijken. Selecteer een aanbeveling of de status van meer details kunnen zien.

Als u wilt weergeven en aanbevelingen toepassen, moet u de machtigingen van de juiste [Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md) in Azure wordt aangegeven. **Lezer**, **SQL DB Inzender** machtigingen zijn vereist voor weergave aanbevelingen en **eigenaar**, **SQL DB Inzender** machtigingen zijn vereist voor het uitvoeren van bewerkingen; maken of sleep indexen en annuleren indexen maken.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **meer services** > **SQL-databases**, en selecteert u de database.
5. Klik op **prestaties aanbeveling** om beschikbaar aanbevelingen voor de geselecteerde database weer te geven.

> [AZURE.NOTE] Als u aanbevelingen een database, moet over een dag van gebruik en moet er enkele activiteit. Ook moet er enkele consistente activiteit. De SQL-Database Advisor kunt eenvoudiger optimaliseren voor consistente query patronen dan deze kan voor willekeurig gevlekte lichtflitsen activiteit. Als aanbevelingen niet beschikbaar zijn, moet de **prestaties aanbeveling** pagina een bericht waarin wordt uitgelegd waarom geven.

![Aanbevelingen](./media/sql-database-advisor-portal/recommendations.png)

Hier volgt een voorbeeld van de aanbeveling ' Schema kunt probleem oplossen door' in de portal van Azure.

![Schema kunt oplossen](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Aanbevelingen zijn gesorteerd op hun mogelijke invloed op prestaties in de volgende vier categorieën:

| Impact | Beschrijving |
| :--- | :--- |
| Hoog | Hoge impact aanbevelingen moeten de belangrijkste invloed op de prestaties te geven. |
| Gemiddeld | Middellange impact aanbevelingen moeten de prestaties verbeteren, maar niet aanzienlijk. |
| Laag | Lage impact aanbevelingen moeten geven betere prestaties dan zonder, maar verbeteringen in de inhoud mogelijk niet aanzienlijk. 


### <a name="removing-recommendations-from-the-list"></a>Aanbevelingen verwijderen uit de lijst

Als uw lijst met aanbevelingen items die u wilt verwijderen uit de lijst bevat, kunt u de aanbeveling verwijderen:

1. Selecteer een aanbeveling in de lijst met **aanbevelingen**.
2. Klik op het blad **Details** op **negeren** .


Desgewenst kunt u verwijderde items terug naar de lijst **aanbevelingen** toevoegen:

1. Klik op het blad **aanbevelingen** op **weergave verwijderd**.
1. Selecteer een verwijderde item in de lijst om de berichtdetails te bekijken.
1. Klik desgewenst op **Ongedaan maken negeren** als u wilt toevoegen van de index terug naar de hoofdlijst met **aanbevelingen**.



## <a name="applying-recommendations"></a>Aanbevelingen toepassen

SQL-Database Advisor biedt u volledige controle over hoe aanbevelingen zijn ingeschakeld met een van de volgende drie opties: 

- Afzonderlijke aanbevelingen één tegelijk toepassen.
- De advisor om toe te passen automatisch aanbevelingen inschakelen (momenteel geldt voor alleen aanbevelingen index).
- Als u wilt een aanbeveling handmatig implementeren, uitgevoerd het aanbevolen T-SQL-script ten opzichte van de database.

Selecteer eventuele aanbeveling de details weergeven en klik vervolgens op **script weergeven** als u wilt bekijken, de exacte details van hoe de aanbeveling wordt gemaakt.

De database blijft online Hoewel de advisor is van toepassing de aanbeveling--gebruik van de SQL-Database Advisor nooit heeft een database offline.

### <a name="apply-an-individual-recommendation"></a>Een afzonderlijke aanbeveling toepassen

U kunt bekijken en aanbevelingen één tegelijk accepteren.

1. Klik op het blad **aanbevelingen** op een aanbeveling.
2. Klik op **toepassen**op het blad **Details** .

    ![Aanbeveling toepassen](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Automatisch indexen management inschakelen

U kunt de SQL-Database Advisor willen implementeren aanbevelingen automatisch instellen. Aanbevelingen beschikbaar komen ze automatisch worden toegepast. Als met alle indexbewerkingen die worden beheerd door de service, als deze prestatieproblemen negatief getal is wordt de aanbeveling worden hersteld.

1. Klik op het blad **aanbevelingen** op **automatiseren**:

    ![Advisor-instellingen](./media/sql-database-advisor-portal/settings.png)

2. De advisor automatisch **maken** of **neerzetten** indexen instellen:

    ![Aanbevolen indexen](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>Handmatig uitvoeren de aanbevolen T-SQL-script

Selecteer een aanbeveling en klik op **script weergeven**. In dit script uitvoeren ten opzichte van de database naar de aanbeveling handmatig toe te passen.

*Indexen die handmatig worden uitgevoerd niet worden gecontroleerd en gevalideerd voor invloed op de prestaties door de service* zodat deze wordt voorgesteld dat u deze indexen na het maken om te controleren of ze bieden prestatieverbeteringen controleren en aanpassen of verwijderen, indien nodig. Zie voor meer informatie over het maken van indexen [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### <a name="canceling-recommendations"></a>Aanbevelingen annuleren

Aanbevelingen met een status **in behandeling**, **controleren**of **succes** kunnen worden geannuleerd. Aanbevelingen met de status van de **uitvoering** , kunnen niet worden geannuleerd.

1. Selecteer een aanbeveling in het gedeelte **Geschiedenis optimaliseren** openen van het blad **aanbevelingen details** .
2. Klik op **Annuleren** om af te breken van het proces van het toepassen van de aanbeveling.



## <a name="monitoring-operations"></a>De controles

Het toepassen van een aanbeveling mogelijk niet onmiddellijk plaatsvindt. De portal details over de status van aanbeveling bewerkingen. Hier volgen statussen die een index in kan zijn:

| Status | Beschrijving |
| :--- | :--- |
| In behandeling | Aanbeveling is ontvangen voor opdrachten en parameters is gepland voor uitvoering toepassen. |
| Uitvoeren | De aanbeveling wordt toegepast. |
| Succes | Aanbeveling is toegepast. |
| Fout | Er is een fout opgetreden tijdens het proces van het toepassen van de aanbeveling. Dit is een tijdelijk probleem of mogelijk een schema wijzigen in de tabel en het script is niet langer geldig. |
| Terug te keren | De aanbeveling is toegepast, maar niet zodat geacht en automatisch wordt omgezet. |
| Teruggezet | De aanbeveling ongedaan is gemaakt. |

Klik op een aanbeveling in het proces in de lijst meer details kunnen zien:

![Aanbevolen indexen](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Een aanbeveling terug te keren

Als u de advisor gebruikt bij het toepassen van de aanbeveling wordt (wat betekent dat u hebt de T-SQL-script niet handmatig uitvoeren) automatisch overgeschakeld deze als dit de invloed op de prestaties negatief worden gevonden. Als voor welke reden dan ook u gewoon wilt wilt terugdraaien van een aanbeveling kunt u het volgende doen:


1. Selecteer een toegepast aanbeveling in het gedeelte **Geschiedenis afstemmen** .
2. Klik op **herstellen** op het blad **aanbeveling details** .

![Aanbevolen indexen](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Controle van invloed op de prestaties van index aanbevelingen

Nadat u aanbevelingen zijn geïmplementeerd (momenteel indexeren bewerkingen en query's aanbevelingen alleen voorzien) kunt u **Query inzichten** op het blad aanbeveling details [Query prestaties inzichten](sql-database-query-performance.md) te openen en ziet u de invloed op de prestaties van de meest voorkomende query's.

![Invloed op de monitor prestaties](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Overzicht

SQL-Database Advisor bevat aanbevelingen voor het verbeteren van de prestaties van de SQL-database. Dankzij de T-SQL-scripts, alsmede afzonderlijke en volledig-automatische (momenteel index alleen), de advisor handig ondersteuning biedt in uw database optimaliseren en uiteindelijk query verbeteren.



## <a name="next-steps"></a>Volgende stappen

Bewaak uw aanbevelingen en gaat u verder met het toepassen om de prestaties te verfijnen. Database-werkbelastingen zijn dynamisch en continu wijzigen. SQL-Database advisor blijft bewaken en om aanbevelingen waarmee u mogelijk de prestaties van uw database kunnen verbeteren. 

 - Zie [SQL Database Advisor](sql-database-advisor.md) voor een overzicht van de SQL-Database Advisor.
 - Zie [Query prestaties inzichten](sql-database-query-performance.md) voor meer informatie over het weergeven van invloed op de prestaties van de meest voorkomende query's.

## <a name="additional-resources"></a>Aanvullende informatie

- [Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
- [INDEXEN MAKEN](https://msdn.microsoft.com/library/ms188783.aspx)
- [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-configure.md)






