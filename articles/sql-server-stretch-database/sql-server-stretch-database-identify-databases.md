<properties
    pageTitle="Welke van databases en tabellen voor uitrekken Database door uitrekken Database Advisor uit te voeren | Microsoft Azure"
    description="Informatie over het identificeren van databases en tabellen die kandidaten voor uitrekken Database zijn."
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
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Welke van databases en tabellen voor uitrekken Database door uitrekken Database Advisor uit te voeren

Als u wilt identificeren databases en tabellen die kandidaten voor uitrekken Database zijn, SQL Server 2016 Upgrade Advisor downloaden en uitrekken Database Advisor uitvoeren. Uitrekken Database Advisor kunt u ook blokkeren problemen identificeren.

## <a name="download-and-install-upgrade-advisor"></a>Download en installeer Upgrade Advisor
Download en installeer Upgrade Advisor van [hier](http://go.microsoft.com/fwlink/?LinkID=613421). Dit hulpmiddel wordt niet opgenomen in de SQL Server-installatiemedia.

## <a name="run-the-stretch-database-advisor"></a>Advisor uitrekken Database uitvoeren

1.  Upgrade Advisor uitvoeren.

2.  Selecteer **scenario's**en selecteer vervolgens **Uitvoeren UITREKKEN DATABASE ADVISOR**.

3.  Klik op het blad **Uitvoeren uitrekken Database Advisor** op **DATABASES selecteren om te analyseren**.

4.  Voer op het blad **databases selecteren** of selecteert u de servernaam van de en de verificatie-informatie. Klik op **verbinding maken**.

5.  Er verschijnt een lijst met databases op de geselecteerde server. Selecteer de databases die u wilt analyseren. Klik op **selecteren**.

6.  Klik op het blad **Uitvoeren uitrekken Database Advisor** op **uitvoeren**.  De analyse wordt uitgevoerd.

## <a name="review-the-results"></a>De resultaten bekijken

1.  Wanneer de analyse is voltooid, klikt u op het blad **Analyzed databases** , selecteert u een van de databases die u als u wilt weergeven van het blad **analyseresultaten** geanalyseerd.

    Het blad **analyseresultaten** bevat aanbevolen tabellen in de geselecteerde database die voldoen aan de criteria van de aanbeveling standaard.

2.  Selecteer in de lijst met tabellen op het blad **analyseresultaten** , een van de aanbevolen tabellen om het blad **tabel resultaten** weer te geven.

    Als er zijn problemen met blokkering, worden het blad **tabel resultaten** de blokkering problemen voor de geselecteerde tabel. Zie [beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md)voor meer informatie over blokkeren gedetecteerd door uitrekken Database Advisor.

3.  In de lijst met blokkeren problemen op het blad **resultaten van de tabel** , selecteert u een van de problemen om meer informatie over het probleem met de geselecteerde weer te geven en risicobeperking stappen voorstelt. Implementeer de voorgestelde risicobeperking stappen als u wilt configureren van de geselecteerde tabel voor uitrekken Database.

## <a name="next-step"></a>Volgende stap
Uitrekken Database inschakelen.

-   Zie voor meer informatie over het inschakelen van uitrekken Database op een **database** [Uitrekken Database inschakelen voor een database](sql-server-stretch-database-enable-database.md).

-   Zie [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md)uitrekken Database op een andere **tabel**, inschakelen wanneer uitrekken al op de database is ingeschakeld.

## <a name="see-also"></a>Zie ook

[Beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md)

[Uitrekken Database inschakelen voor een database](sql-server-stretch-database-enable-database.md)

[Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md)

[Alle onderwerpen voor de service Azure uitrekken SQL Server-Database](sql-server-stretch-database-index-all-articles.md)
