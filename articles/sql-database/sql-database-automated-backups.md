<properties
   pageTitle="Back-ups van SQL-Database - automatische, geografische-redundante | Microsoft Azure" 
   description="SQL-Database Hiermee maakt u een lokale database back-up om de vijf minuten automatisch en wordt geografische-redundante opslag in Azure leestoegang (AB-GRS) om redundantie geografische. "
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
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Meer informatie over het back-ups van SQL-Database

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

SQL-Database Hiermee maakt u een lokale database back-up om de paar minuten automatisch en Azure leestoegang geografische-redundante opslag voor geografische redundantie wordt gebruikt. Back-ups zijn een essentieel onderdeel van een strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel, omdat ze uw gegevens tegen ongeluk beschadigde bestanden of verwijdering beschermen. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Wat is een back-up van SQL-Database?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

Een SQL-Database back-up bevat zowel lokale back-ups en geografische-redundante back-ups. Deze back-ups worden gemaakt automatisch en zonder bijkomende kosten. U hoeft niet te iets doen om ze optreden.

<!----------------- 
    Explains first component of the backup feature
------------------>

Lokale back-ups gebruikt SQL-Database van SQL Server-technologie [volledige](https://msdn.microsoft.com/library/ms186289.aspx), [afwijkingen](https://msdn.microsoft.com/library/ms175526.aspx )en [transactielogboek](https://msdn.microsoft.com/library/ms191429.aspx) back-ups maken. De back-ups log transactie optreden elke vijf minuten, waarmee u moet een punt-in-tijd op de server waarop de database herstellen. Wanneer u een database terugzet, wordt de service uit welke volledige, verschil en transactie log back-ups moeten worden teruggezet cijfers.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Een lokale database back-up om te gebruiken:

- Een database terugzetten naar een punt in de tijd binnen de bewaarperiode. Met een back-up van database kunt u een database terugzetten naar een punt in de tijd, een verwijderde database terugzetten naar de tijd die is verwijderd of een database terugzetten naar een ander geografisch gebied. Als u wilt een herstellen uitvoeren, raadpleegt u [een database uit een back-up van database terugzetten](sql-database-recovery-using-backups.md).

- Een database met een SQL server in de regio dezelfde of een andere kopiëren. De kopie is transactioneel consistent is met de huidige SQL-Database. Als u wilt een kopie uitvoeren, raadpleegt u [database kopiëren](sql-database-copy.md).

- Een database back-up voorbij de back-up bewaarperiode archiveren. Om uit te voeren een archief, het [exporteren van een SQL-database naar een Bacpac-](sql-database-export.md) bestand. U kunt vervolgens de BACPAC met langdurige storage archiveren en buiten uw bewaarperiode opslaan. Of de BACPAC gebruiken om over te brengen van een kopie van uw database naar SQL Server, een on-premises implementatie of in een Azure virtuele machine (VM).

<!----------------- 
    Explains first component of the backup feature
------------------>

Geografische-redundante back-ups gebruikt SQL-Database [Azure Storage herhaling](../storage/storage-redundancy.md). Lokale database back-upbestanden SQL-Database opgeslagen in een account [Leestoegang geografische-redundante opslag (AB-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) . Azure wordt overgenomen door de back-upbestanden een [gepaarde Datacenter](../best-practices-availability-paired-regions.md). 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Gebruik een geografische-redundante back-up naar:

- Als u geen toegang de database back-up van uw primaire databaseregio tot, moet u een database terugzetten naar een ander geografisch gebied. 

>[AZURE.NOTE] In Azure opslag, wordt de term *herhaling* verwijst naar het kopiëren van bestanden van de ene locatie naar een andere. De SQL- *databasereplicatie* verwijst naar het behouden tot meerdere secundaire databases die zijn gesynchroniseerd met een primaire-database. 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Hoeveel opslagruimte back-up is opgenomen gratis?

SQL-Database biedt maximaal 200% van uw opslag maximale ingerichte database als back-up opslaan zonder extra kosten. Bijvoorbeeld als u een exemplaar van de standaard DB met een ingerichte DB grootte van 250 GB hebt, hebt u 500 GB van back-up opslaan zonder bijkomende kosten. Als uw database groter is dan de meegeleverde back-up opslaan, kunt u kiezen of u de bewaarperiode verkleinen door het contact opneemt met Azure-ondersteuning. Er is een andere optie te betalen voor extra back-up opslaan dat wordt gefactureerd bij het standaardtarief van leestoegang geografisch redundante opslag (AB-GRS). 

## <a name="how-often-do-backups-happen"></a>Hoe vaak back-ups plaats?

Back-ups van lokale database, worden volledige back-ups wekelijks optreden differentiële back-ups optreden per uur en transactielogboek back-ups optreden om de vijf minuten. De eerste volledige back-up is gepland onmiddellijk nadat u een database is gemaakt. Meestal is voltooid binnen 30 minuten, maar kan het langer duren wanneer de database van een aanzienlijk grootte is. De eerste back-up kunt bijvoorbeeld duurt langer op een herstelde database of een databasekopie. Na de eerste volledige back-up, worden alle verdere back-ups automatisch gepland en beheerd stilte op de achtergrond. De exacte timing van volledige en [differentiële](https://msdn.microsoft.com/library/ms175526.aspx) back-ups wordt bepaald volgens deze de algehele systeem werklast saldi. 

Geografische-redundante back-ups, worden de back-ups van een volledige en differentiële volgens de planning van Azure Storage herhaling gekopieerd.

## <a name="how-long-do-you-keep-my-backups"></a>Hoe lang houdt u Mijn back-ups?

Elke SQL-Database back-up heeft een bewaarperiode die is gebaseerd op de [service niveaus](sql-database-service-tiers.md) van de database. De bewaarperiode voor een database in de:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Eenvoudige servicelaag is zeven dagen.
- Standaard servicelaag is 35 dagen.
- Premium servicelaag is 35 dagen.


Als u een database van de standaard- of Premium service niveaus worden eenvoudige downgraden, worden de back-ups gedurende zeven dagen opgeslagen. Alle bestaande back-ups ouder zijn dan zeven dagen zijn niet langer beschikbaar. 

Als u uw database vanuit de laag Basic-service in standaard of Premium bijwerkt, blijft SQL-Database bestaande back-ups totdat ze 35 dagen oud zijn. Deze blijft nieuwe back-ups schiet voor 35 dagen.
 
Als u een database verwijdert, blijft SQL-Database de back-ups op dezelfde manier zou voor een online-database. Stel dat u een eenvoudige database met een periode van zeven dagen verwijderen. Een back-up die vier dagen oud is opgeslagen voor drie dagen.

>[AZURE.IMPORTANT]
    Als u de Azure SQL-server die als host fungeert voor SQL-Databases verwijdert, worden alle databases die deel uitmaakt van de server worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Volgende stappen

Back-ups zijn een essentieel onderdeel van een strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel, omdat ze uw gegevens tegen ongeluk beschadigde bestanden of verwijdering beschermen. Om te zien hoe back-ups-database naar een uitgebreidere strategie, Zie [bedrijven bedrijfscontinuïteit overzicht](sql-database-business-continuity.md).


